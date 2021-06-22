<template>
  <bubble-canvas :img-url="dragonImage"></bubble-canvas>
  <mosaic-canvas :img-url="flowerImage"></mosaic-canvas>
</template>

<script>
import BubbleCanvas from './components/BubbleCanvas.vue';
import MosaicCanvas from './components/MosaicCanvas.vue';

export default {
  name: 'App',
  components: {
    BubbleCanvas,
    MosaicCanvas
  },
  setup() {
    const dragonImage = require('@/assets/dragon.jpg');
    const flowerImage = require('@/assets/flower.jpg');

    return {
      dragonImage,
      flowerImage
    }
  }
}
</script>

<style>
</style>
