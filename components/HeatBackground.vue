<script setup>
import { ref, onMounted, onUnmounted } from "vue";
const canvas = ref(null);
let animationId = null;

const width = 980;
const height = 552;

onMounted(() => {
  const ctx = canvas.value.getContext("2d");
  let time = 0;

  function draw() {
    // 1. Clear with transparency instead of solid white
    ctx.clearRect(0, 0, width, height);

    ctx.beginPath();
    ctx.strokeStyle = "rgba(239, 68, 68, 0.3)"; // Subtle Red
    ctx.lineWidth = 2;

    for (let x = 0; x < width; x++) {
      const mid = width / 2;
      const sigma = 60 + time * 1.5;
      const amplitude = 100 * (60 / sigma);
      
      // 2. Positioned at the bottom (height * 0.8) to leave room for text
      const baseline = height * 0.85; 
      const y = baseline - amplitude * Math.exp(-Math.pow(x - mid, 2) / (2 * Math.pow(sigma, 2)));
      
      if (x === 0) ctx.moveTo(x, y);
      else ctx.lineTo(x, y);
    }

    ctx.stroke();
    
    // Add a subtle "floor" line
    ctx.beginPath();
    ctx.strokeStyle = "rgba(0, 0, 0, 0.05)";
    ctx.moveTo(0, height * 0.85);
    ctx.lineTo(width, height * 0.85);
    ctx.stroke();

    time = (time + 0.8) % 400; 
    animationId = requestAnimationFrame(draw);
  }
  draw();
});

onUnmounted(() => cancelAnimationFrame(animationId));
</script>

<template>
  <canvas ref="canvas" width="980" height="552" class="bg-canvas"></canvas>
</template>

<style scoped>
.bg-canvas {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1; /* Placed behind content */
  pointer-events: none;
}
</style>