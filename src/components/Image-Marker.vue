<template>
  <div class="page_content">
    <div class="cell">
      <div class="toolbar">
        <div class="toolbar_left">
          <div
            class="toolbar_btn btn_grey"
            id="add_pos_marker"
            @click="addLeftMarker"
          >
            Add Positive dot
          </div>
          <div
            class="toolbar_btn btn_grey"
            id="add_neg_marker"
            @click="addRightMarker"
          >
            Add Negative dot
          </div>
        </div>
      </div>
    </div>
    <!-- Body -->
    <div class="image-marker-container">
      <!-- Left -->
      <div
        class="image-marker-container__box image-marker-container__box--left"
      >
        <div class="image-marker-container__box__content">
          <template
            v-for="(pos, i) in list.filter((it) => it.col == 1)"
            :key="i"
          >
            <div
              :ref="`textBox${pos.no}`"
              class="image-marker__text-box green ui-draggable ui-draggable-handle"
              style="visibility: visible; position: relative"
            >
              <p
                class="image-marker__text-box__content"
                contenteditable="true"
                @input="(event) => onInput(event, pos)"
              >
                {{ pos.content }}
              </p>

              <div
                class="image-marker__text-box__close-btn"
                @click="removeComment(pos)"
              >
                X
              </div>
            </div>
          </template>
        </div>
      </div>

      <!-- Right -->
      <div
        class="image-marker-container__box image-marker-container__box--right"
      >
        <div class="image-marker-container__box__content">
          <template
            v-for="(neg, i) in list.filter((it) => it.col == 2)"
            :key="i"
          >
            <div
              :ref="`textBox${neg.no}`"
              class="image-marker__text-box yellow ui-draggable ui-draggable-handle"
              style="visibility: visible; position: relative"
            >
              <p
                class="image-marker__text-box__content"
                contenteditable="true"
                @input="(event) => onInput(event, neg)"
              >
                {{ neg.content }}
              </p>
              <div
                class="image-marker__text-box__close-btn"
                @click="removeComment(neg)"
              >
                X
              </div>
            </div>
          </template>
        </div>
      </div>

      <!-- Image -->
      <div
        class="image-marker-container__box image-marker-container__box--image"
        ref="dragArea"
      >
        <img class="image-marker-container__img" :src="imgSrc" />
        <div
          class="image-marker-container__box image-marker-container__box--drag"
        >
          <template v-for="(it, index) of list" :key="index">
            <div
              :ref="`line${it.no}`"
              :class="`image-marker__line ${it.col == 1 ? 'green' : 'yellow'}`"
            ></div>
            <div
              :id="`dotMove${it.no}`"
              :ref="`dot${it.no}`"
              :class="`image-marker__dot ${
                it.col == 1 ? 'green' : 'yellow'
              } ui-draggable ui-draggable-handle`"
              @mousedown="(e) => dragMouseDown(e, it)"
            ></div>
          </template>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import $ from 'jquery'

export default {
  name: 'ImageMarker',
  model: {
    prop: 'modelValue',
    event: 'mousedown',
  },
  props: ['modelValue', 'imgSrc'],

  data() {
    return {
      list: [],
    }
  },

  watch: {
    modelValue: {
      handler(details) {
        this.list = []
        if (details?.length) this.list = details
      },
      immediate: true,
    },
    list: {
      handler(details) {
        this.$nextTick(() => {
          if (details?.length) {
            for (let it of details) {
              this.addCommentMarker(it)
            }
          }
          this.$nextTick(() => {
            // store data
            // window.localStorage.markers = JSON.stringify(details)
            this.$emit('update:modelValue', details)
          })
        })
      },
      immediate: true,
      deep: true,
    },
  },

  methods: {
    // Method
    addLeftMarker() {
      const no = this.maxNo()
      const marker = {
        no,
        content: 'Comment',
        col: 1,
      }
      this.list.push(marker)
    },
    addRightMarker() {
      const no = this.maxNo()
      const marker = {
        no,
        content: 'Comment',
        col: 2,
      }
      this.list.push(marker)
    },

    maxNo() {
      let ls = this.list.map((it) => it.no)
      if (!ls?.length) return 1

      let res = Math.max(...ls) || 0

      return res + 1
    },
    removeComment(marker) {
      const no = marker.no
      const index = this.list.findIndex((it) => it.no == no)

      this.list.splice(index, 1)
    },
    onInput(e, marker) {
      marker.content = e.target.innerText
    },

    dragMouseDown(e, marker) {
      let dragoffset = { x: 0, y: 0 }
      const parent = this.$refs['dragArea']
      // event
      e.preventDefault()
      let el = this.$refs[`dot${marker.no}`][0]

      let pageX =
        e.pageX ||
        e.clientX +
          (document.documentElement.scrollLeft
            ? document.documentElement.scrollLeft
            : parent.scrollLeft)
      let pageY =
        e.pageY ||
        e.clientY +
          (document.documentElement.scrollTop
            ? document.documentElement.scrollTop
            : parent.scrollTop)
      dragoffset.x = pageX - el.offsetLeft
      dragoffset.y = pageY - el.offsetTop

      // get the mouse cursor position at startup:
      document.onmousemove = (e) => this.elementDrag(e, marker, dragoffset)
      document.onmouseup = this.closeDragElement
    },

    elementDrag(e, marker, dragoffset) {
      // Limit draggable area
      const { offsetX, offsetY } = this.limitDraggable(e, marker, dragoffset)

      marker.pos = {
        x: offsetX,
        y: offsetY,
        // x: dot.offsetLeft + e.movementX,
        // y: dot.offsetTop + e.movementY,
      }

      //
      this.addCommentMarker(marker)
    },

    addCommentMarker(marker) {
      const index = marker.no
      const textBox = this.$refs[`textBox${index}`]
      let dot = this.$refs[`dot${index}`]
      let line = this.$refs[`line${index}`]

      if (!textBox?.length || !dot?.length || !line?.length) return false

      line = line[0]
      dot = dot[0]
      const $leftBox = $(
        document.querySelector('.image-marker-container__box__content')
      )
      const $textBox = $(textBox[0])
      const $dot = $(dot)
      const $line = $(line)

      const onDrag = function () {
        // set the element's new position:
        const textBoxOffset =
          $textBox.parent()[0] == $leftBox[0]
            ? $textBox.parent().width() - 4
            : 4

        let x1 = $textBox.offset().left + textBoxOffset
        let y1 = $textBox.offset().top + $textBox.height() / 2
        let x2 = $dot.offset().left + $dot.width() / 2
        let y2 = $dot.offset().top + $dot.height() / 2

        const hypotenuse = Math.sqrt(
          (x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2)
        )
        const angle = Math.atan2(y1 - y2, x1 - x2) * (180 / Math.PI)
        const transform = `rotate(${angle}deg)`

        if (angle >= 90 && angle < 180) {
          y1 = y1 - (y1 - y2)
        }
        if (angle > 0 && angle < 90) {
          x1 = x1 - (x1 - x2)
          y1 = y1 - (y1 - y2)
        }
        if (angle <= 0 && angle > -90) {
          x1 = x1 - (x1 - x2)
        }

        $line
          .queue(function () {
            $(this).offset({ top: y1, left: x1 })
            $(this).dequeue()
          })
          .queue(function () {
            $(this).width(hypotenuse)
            $(this).dequeue()
          })
        // .queue(function () {
        //   $(this).rotate(angle)
        //   $(this).dequeue()
        // })
        line.style.transform = transform
      }

      this.mountTo($leftBox, $textBox, $dot, onDrag, marker)
    },
    mountTo($leftBox, $textBox, $dot, onDrag, marker) {
      if (!marker.pos) {
        const dragArea = $(this.$refs['dragArea'])
        marker.pos = {
          x: $textBox.parent()[0] == $leftBox[0] ? 0 : dragArea.width() - 20,
          y:
            $textBox.offset().top -
            $textBox.parent().offset().top +
            $textBox.height() / 2 -
            10,
        }
      }

      $dot.css({ top: `${marker.pos.y}px`, left: `${marker.pos.x}px` })

      onDrag()
      onDrag()
    },
    limitDraggable(e, marker, dragoffset) {
      let dot = this.$refs[`dot${marker.no}`][0]
      // limit draggable area
      const parent = this.$refs['dragArea']
      let pageX =
        e.pageX ||
        e.clientX +
          (document.documentElement.scrollLeft
            ? document.documentElement.scrollLeft
            : parent.scrollLeft)
      let pageY =
        e.pageY ||
        e.clientY +
          (document.documentElement.scrollTop
            ? document.documentElement.scrollTop
            : parent.scrollTop)
      // left/right constraint
      let offsetX = 0,
        offsetY = 0
      if (pageX - dragoffset.x < 0) {
        offsetX = 0
      } else if (pageX - dragoffset.x + dot.clientWidth > parent.clientWidth) {
        offsetX = parent.clientWidth - dot.clientWidth
      } else {
        offsetX = pageX - dragoffset.x
      }

      // top/bottom constraint
      if (pageY - dragoffset.y < 0) {
        offsetY = 0
      } else if (pageY - dragoffset.y + dot.clientWidth > parent.clientHeight) {
        offsetY = parent.clientHeight - dot.clientWidth
      } else {
        offsetY = e.pageY - dragoffset.y
      }

      return { offsetX, offsetY }
    },
    closeDragElement() {
      document.onmouseup = null
      document.onmousemove = null
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.image-marker-container {
  width: 100%;
  height: 500px;
  overflow: hidden;
  position: relative;
}

.image-marker-container__box {
  position: absolute;
  border: 1px solid #ddd;
  top: 1px;
  bottom: 1px;
}

.image-marker-container__box__content {
  position: relative;
}

.image-marker-container__box--left {
  left: 0;
  width: 23%;
}

.image-marker-container__box--right {
  right: 0;
  width: 23%;
}

.image-marker-container__box--image {
  left: 27%;
  right: 27%;
  background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAKCAYAAACNMs+9AAAAQUlEQVQYV2NkIABWrVr1H6SEEZ86mKKwsDBGnAqRFeE0EV0RVoXYFGEoxKUIRSE+RXCFhBSBFRKjCK4QFE6EwhMA6fIqBUB9cUAAAAAASUVORK5CYII=);
  display: flex;
  align-items: center;
  justify-content: center;
}

.image-marker-container__img {
  display: block;
  max-width: 100%;
  max-height: 100%;
}

.image-marker-container__box--drag {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  border: none;
}

.image-marker__text-box {
  margin-bottom: 10px;
}

.image-marker__text-box__close-btn {
  position: absolute;
  right: 3px;
  top: 3px;
  cursor: pointer;
  display: none;
}

.image-marker__text-box:hover .image-marker__text-box__close-btn {
  display: block;
}

.image-marker-container__box--left .image-marker__text-box {
  border-width: 6px;
  border-style: solid;
  border-top: 0;
  border-left: 0;
  border-bottom: 0;
}

.image-marker-container__box--right .image-marker__text-box {
  border-width: 6px;
  border-style: solid;
  border-top: 0;
  border-right: 0;
  border-bottom: 0;
}

.image-marker__text-box__title {
  font-family: 'Source Sans Pro', sans-serif;
  font-size: 14px;
  font-weight: 600;
  color: #444;
  padding: 10px;
  padding-bottom: 0;
  margin: 0;
}

.image-marker__text-box__content {
  font-family: 'Source Sans Pro', sans-serif;
  font-size: 14px;
  font-weight: 300;
  color: #444;
  padding: 10px;
  margin: 0;
  line-height: 18px;
}

.image-marker__dot {
  width: 18px;
  height: 18px;
  cursor: move;
  border-radius: 50%;
  position: absolute;
}

.image-marker__line {
  height: 2px;
  position: absolute;
}
/* === */
.page_content {
  width: 960px;
  margin: 150px auto;
  position: relative;
  overflow: auto;
}

.toolbar {
  min-height: 32px;
  margin: 9px 0 9px 10px;
  line-height: 32px;
  font-size: 16px;
  overflow: auto;
}

.toolbar_btn {
  float: left;
  padding: 0 10px;
  line-height: 30px;
  margin-right: 10px;
  border-radius: 6px;
  color: #444;
  cursor: pointer;
  background-repeat: no-repeat;
}

.btn_grey {
  background-color: #ddd;
  margin: 1px 10px 0 0;
  height: 30px;
}

.btn_grey:hover {
  color: #fff;
  background-color: #1986c4;
  border: 0;
}

.yellow {
  background-color: rgba(255, 145, 0, 0.15);
  border-color: #ff9100;
}

.green {
  background-color: rgba(125, 189, 59, 0.15);
  border-color: #7dbd3b;
}

.image-marker__dot.yellow,
.image-marker__line.yellow {
  background-color: #ff9100;
}

.image-marker__dot.green,
.image-marker__line.green {
  background-color: #7dbd3b;
}
</style>
