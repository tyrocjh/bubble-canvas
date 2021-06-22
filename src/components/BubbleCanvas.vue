<template>
  <canvas ref="bubbleCanvas"></canvas>
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
    const bubbleCanvas = ref(null);

    onMounted(() => {
      drawBubble();
    })

    // 画图
    const drawBubble = () => {
      const canvas = bubbleCanvas.value;
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

        // 清空画布
        ctx.fillStyle = '#ffffff';
        ctx.fillRect(0, 0, image.width, image.height);

        // 根据imageData画像素点
        const gap = 6;
        for (let h=0; h<image.height; h+=gap) {
          for (let w=0; w<image.width; w+=gap) {
            const position = (h * image.width + w) * 4;
            const r = imageData[position], g = imageData[position + 1], b = imageData[position + 2];
            if (r<50 && g<50 && b<50) {
              ctx.fillStyle = '#000000';
              ctx.fillRect(w, h, 4, 4);
            }
          }
        }
      }
    }

    return {
      bubbleCanvas
    }
  },
};
</script>
