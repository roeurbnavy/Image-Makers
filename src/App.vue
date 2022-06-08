<template>
  <HelloWorld v-model="items" />
  <!-- <DivDraggableVue /> -->
  <!-- <DrawLineVue /> -->
  <ImageMarker v-model="init" />
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
// import DivDraggableVue from './components/DivDraggable.vue'
// import DrawLineVue from './components/DrawLine.vue'
import ImageMarker from './components/Image-Marker.vue'
export default {
  name: 'App',
  components: {
    HelloWorld,
    // DivDraggableVue,
    // DrawLineVue,
    ImageMarker,
  },
  data() {
    return {
      items: 'Hi',
      init: [
        {
          className: 'yellow',
          col: 2,
          content: '14',
          no: 2,
          pos: { x: 320, y: 145 },
        },
        {
          className: 'yellow',
          col: 2,
          content: '9',
          no: 4,
          pos: { x: 234, y: 41 },
        },
        {
          className: 'green',
          col: 1,
          content: '31',
          no: 3,
          pos: { x: 101, y: 305 },
        },
        {
          className: 'green',
          col: 1,
          content: '1',
          no: 1,
          pos: {
            x: 99,
            y: 228,
          },
        },
      ],
    }
  },
  watch: {
    init: {
      handler(val) {
        console.log('parent', val)
      },
      immediate: true,
      deep: true,
    },
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
