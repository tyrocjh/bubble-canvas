<template>
  <canvas ref="mosaicCanvas"></canvas>
</template>

<script>
import { ref, onMounted } from '@vue/runtime-core';

export default {
  props: {
    imgUrl: {
      // 图片链接
      required: true,
      type: String
    }
  },
  setup(props) {
    const mosaicCanvas = ref(null);

    onMounted(() => {
      drawMosaic();
    })

    // 获取随机整数
    const randomIntegerInRange = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;

    // 像素点颜色数组
    const bgArr = ['rgba(204, 204, 204, 0.5)', 'rgba(144, 144, 144, 0.5)', 'rgba(80, 80, 80, 0.5)'];

    // 画图
    const drawMosaic = () => {
      const canvas = mosaicCanvas.value;
      const ctx = canvas.getContext('2d');

      let image = new Image();
      image.src = props.imgUrl;
      image.onload = () => {
        canvas.width = image.width;
        canvas.height = image.height;

        // 画图片
        ctx.drawImage(image, 0, 0);
        // 获取图片像素点数组
        const imageData = ctx.getImageData(0, 0, image.width, image.height).data;

        // 根据imageData画像素点
        const gap = 10;
        for (let h=0; h<image.height; h+=gap) {
          for (let w=0; w<image.width; w+=gap) {
            const position = (h * image.width + w) * 4;
            const r = imageData[position], g = imageData[position + 1], b = imageData[position + 2];
            if (r>130 && g<80 && b<100) {
              ctx.fillStyle = bgArr[randomIntegerInRange(0, 2)];
              ctx.fillRect(w - 10, h - 10, 10, 10);
            }
          }
        }
      }
    }

    return {
      mosaicCanvas
    }
  },
};
</script>
