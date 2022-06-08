<template>
  <!-- <HelloWorld v-model="items" /> -->
  <!-- <DivDraggableVue /> -->
  <!-- <DrawLineVue /> -->
  <ImageMarker v-model="init" :img-src="imgSrc" />
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
// import DivDraggableVue from './components/DivDraggable.vue'
// import DrawLineVue from './components/DrawLine.vue'
import ImageMarker from './components/Image-Marker.vue'
export default {
  name: 'App',
  components: {
    // HelloWorld,
    // DivDraggableVue,
    // DrawLineVue,
    ImageMarker,
  },
  data() {
    return {
      items: 'Hi',
      imgSrc:
        'https://upload.wikimedia.org/wikipedia/commons/thumb/6/6c/Universal_Numbering_System.svg/1200px-Universal_Numbering_System.svg.png',
      init: [
        {
          col: 2,
          content: '14',
          no: 2,
          pos: { x: 320, y: 145 },
        },
        {
          col: 2,
          content: '9',
          no: 4,
          pos: { x: 234, y: 41 },
        },
        {
          col: 1,
          content: '31',
          no: 3,
          pos: { x: 101, y: 305 },
        },
        {
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
