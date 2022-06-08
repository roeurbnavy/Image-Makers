<template>
  <div class="content">
    <!-- @mousedown="(e) => mouseDown(e, '#box1')" -->
    <div class="box" id="box1"></div>
    <!-- <div class="image-marker"> -->
    <div @mousedown="(e) => mouseDown(e, '#box2')" class="box" id="box2"></div>

    <div class="line" id="line"></div>
    <!-- </div> -->
  </div>
</template>
<script>
//Ref : https://www.youtube.com/watch?v=LgTrwpMCww8
export default {
  name: 'DrawLine',
  mounted() {
    this.onDrag()
    // this.moveLine(10, 30, 300, 500)
  },
  methods: {
    mouseDown(e, id) {
      document.onmousemove = (e) => this.onDrag(e, id)
      document.onmouseup = this.closeDragElement
    },
    onDrag(evt, id) {
      if (evt?.movementX) {
        const wrapper = document.querySelector(id)
        const box = wrapper.getBoundingClientRect()
        // let getStyle = window.getComputedStyle(wrapper)
        // wrapper.getBoundingClientRect()
        // let leftVal = parseInt(getStyle.left)
        // let topVal = parseInt(getStyle.top)
        // let leftVal = getStyle.left
        // let topVal = getStyle.top
        wrapper.style.left = `${wrapper.offsetLeft + evt.movementY}px`
        // `${leftVal + evt.movementX}px`
        wrapper.style.top = `${wrapper.offsetTop + evt.movementX}`
        // `${topVal + evt.movementY}px`

        // box1.style.left = '15px'
        // box1.style.top = '20px'
      }

      // x = left,y = top
      // const content = document.querySelector('.content')
      const box1 = document.querySelector('#box1')
      const box2 = document.querySelector('#box2')
      const b1 = this.getOffset(box1)
      const b2 = this.getOffset(box2)
      const parent = this.getOffset(box1.parentNode)
      console.log('parentNode', parent, box1.parentNode)

      // const x1 = b1.left + b1.width
      // const y1 = b1.top + b1.height

      // const x2 = b2.left + b1.width
      // const y2 = b2.top

      // const b1 = box1.getBoundingClientRect()
      // const b2 = box2.getBoundingClientRect()

      let relativePos = {}

      relativePos.top = b1.top - parent.top
      relativePos.right = b1.right - parent.right
      relativePos.bottom = b1.bottom - parent.bottom
      relativePos.left = b1.left - parent.left

      console.log(relativePos)
      const x1 = b1.left
      // + parent.width
      // + parent.width // x
      // (b1.left + b1.width) / 2
      const y1 = b1.top + b1.height / 2
      // + b1.height / 2 // y
      // (b1.top + b1.height) / 2

      const x2 = b2.left + b2.width / 2 // x
      // (b2.left + b1.width) / 2
      const y2 = b2.top + b2.height / 2 //
      // (b2.top + b1.height) / 2
      console.log('x1=', x1, ',y1=', y1, ',x2=', x2, ',y2=', y2)
      // const wrapper2 = document.querySelector('#box2')
      // let getStyle2 = window.getComputedStyle(wrapper1)

      this.moveLine(x1, y1, x2, y2)
    },
    moveLine(x1, y1, x2, y2) {
      // const line = document.querySelector('#line')
      const line = document.getElementById('line')

      const length = Math.sqrt((x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2))
      const angle = Math.atan2(y1 - y2, x1 - x2) * (180 / Math.PI)
      // (Math.atan2(y2 - y1, x2 - x1) * 180) / Math.PI
      const transform = `rotate(${angle}deg)`
      // console.log('length', length, x1, y1, x2, y2)
      const offsetX =
        // x1
        (x1 + x2) / 2 - length / 2
      // x1 > x2 ? x2 : x1
      const offsetY =
        // y1
        (y1 + y2) / 2
      // y1 > y2 ? y2 : y1

      line.style.top = `${offsetY}px`
      line.style.left = `${offsetX}px`
      line.style.width = `${length}px`
      line.style['-webkit-transform'] = `${transform}`
      line.style['-moz-transform'] = `${transform}`
      line.style.transform = `${transform}`
    },
    getOffset(el) {
      const rect = el.getBoundingClientRect()
      return rect
      // console.log('rect', rect, el)
      // return {
      //   // left: rect.left + window.pageXOffset,
      //   // top: rect.top + window.pageYOffset,
      //   // width: rect.width || el.offsetWidth,
      //   // height: rect.height || el.offsetHeight,
      //   left: rect.left + window.pageXOffset,
      //   top: rect.top + window.pageYOffset,
      //   right: rect.right + window.pageXOffset,
      //   bottom: rect.bottom + window.pageYOffset,
      //   width: rect.width || el.offsetWidth,
      //   height: rect.height || el.offsetHeight,
      // }
    },
    closeDragElement() {
      document.onmouseup = null
      document.onmousemove = null
    },
  },
}
</script>
<style>
.content {
  width: 100%;
  height: 500px;
  position: relative;
  /* border: 1px solid black; */
  /* background-color: goldenrod; */
  overflow: hidden;
}
.image-marker {
  position: relative;
  /* background-color: aquamarine; */
  border: 1px solid black;
  width: 100%;
  height: 500px;
  left: 20%;
  right: 20%;
}
.box {
  border: 1px solid black;
  background-color: #ccc;
  /* width: 50px;
  height: 50px; */
  /* width: 4px;
  height: 4px; */
  position: absolute;
}

#line1 {
  /* top: 100px;
  left: 50px; */
  /*transform: rotate(222deg);
    -webkit-transform: rotate(222deg);
    -ms-transform: rotate(222deg);*/
}

.line {
  /* width: 1px; */
  height: 2px;
  background-color: black;
  position: absolute;
  /* z-index: -1; put line behind the boxes */
}

#box1 {
  /* top: 0;
  left: 0; */
  width: 30px;
  height: 30px;
  left: 10px;
  top: 30px;
  background-color: red;
}

#box2 {
  /* top: 200px;
  left: 0; */
  left: 300px;
  top: 200px;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  /* position: absolute; */
  background-color: green;
}
</style>
