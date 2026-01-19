<script setup>
import { ref, onMounted, onUnmounted } from "vue";
const canvas = ref(null);
let animationId = null;

onMounted(() => {
  const ctx = canvas.value.getContext("2d");
  let time = 0;

  function draw() {
    ctx.fillStyle = "white";
    ctx.fillRect(0, 0, 980, 552);

    ctx.beginPath();
    ctx.strokeStyle = "rgba(239, 68, 68, 0.4)"; // Red
    ctx.lineWidth = 3;

    for (let x = 0; x < 980; x++) {
      const mid = 490;
      // Fundamental solution of heat equation: Gaussian that widens with time
      const sigma = 50 + time * 2;
      const amplitude = 150 * (50 / sigma);
      const y = 276 - amplitude * Math.exp(-Math.pow(x - mid, 2) / (2 * Math.pow(sigma, 2)));
      
      if (x === 0) ctx.moveTo(x, y);
      else ctx.lineTo(x, y);
    }

    ctx.stroke();
    time = (time + 1) % 500; // Reset after it flattens
    animationId = requestAnimationFrame(draw);
  }
  draw();
});

onUnmounted(() => cancelAnimationFrame(animationId));
</script>

<template><canvas ref="canvas" width="980" height="552" class="bg-canvas"></canvas></template>