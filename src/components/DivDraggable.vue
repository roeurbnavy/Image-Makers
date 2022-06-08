<template>
  <div>
    <h1>Div Draggle</h1>
    <div ref="wrapper" id="mydiv">
      <div id="mydivheader" @mousedown="mouseDown">Click here to move</div>
      <p>Move</p>
      <p>this</p>
      <p>DIV</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DivDraggable',
  methods: {
    mouseDown() {
      document.onmousemove = this.onDrag
      document.onmouseup = this.closeDragElement
    },
    onDrag({ movementX, movementY }) {
      const wrapper = this.$refs['wrapper']
      // document.querySelector('#mydiv')
      // header = wrapper.querySelector('header')

      let getStyle = window.getComputedStyle(wrapper)
      console.log('getStyle', getStyle)
      let leftVal = parseInt(getStyle.left)
      let topVal = parseInt(getStyle.top)
      wrapper.style.left = `${leftVal + movementX}px`
      wrapper.style.top = `${topVal + movementY}px`
    },
    closeDragElement() {
      document.onmouseup = null
      document.onmousemove = null
    },
  },
}
</script>

<style>
#mydiv {
  position: absolute;
  z-index: 9;
  background-color: #f1f1f1;
  text-align: center;
  border: 1px solid #d3d3d3;
}

#mydivheader {
  padding: 10px;
  cursor: move;
  z-index: 10;
  background-color: #2196f3;
  color: #fff;
}
</style>
