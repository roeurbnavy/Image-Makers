<template>
  <div>
    <input
      type="text"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
    />
    <!-- <label> First Name </label>
    <input
      type="text"
      placeholder="Input"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
    /> -->
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: ['modelValue'],
  model: {
    props: 'modelValue',
    event: 'input',
  },
  // data() {
  //   return {}
  // },
  watch: {
    modelValue: {
      handler(val) {
        console.log('child', val)
      },
      immediate: true,
    },
  },
  mounted() {
    // this.dragMouseDown()
  },
  methods: {
    // change(value) {
    //   // console.log('value', value)
    //   this.$emit('update:modelValue', value)
    // },
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
  width: 20px;
  height: 20px;
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

.yello {
  background-color: rgba(255, 145, 0, 0.15);
  border-color: #ff9100;
}

.green {
  background-color: rgba(125, 189, 59, 0.15);
  border-color: #7dbd3b;
}

.image-marker__dot.yello,
.image-marker__line.yello {
  background-color: #ff9100;
}

.image-marker__dot.green,
.image-marker__line.green {
  background-color: #7dbd3b;
}
</style>
