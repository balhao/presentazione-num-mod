<script setup>
import { ref, onMounted, onUnmounted } from "vue";
const canvas = ref(null);
let animationId = null;

const width = 980;
const height = 552;

onMounted(() => {
  const ctx = canvas.value.getContext("2d");
  let offset = 0;

  function draw() {
    ctx.fillStyle = "rgba(255, 255, 255, 1)";
    ctx.fillRect(0, 0, width, height);

    ctx.beginPath();
    ctx.lineWidth = 2;
    ctx.strokeStyle = "rgba(59, 130, 246, 0.5)"; // Blue

    for (let x = 0; x < width; x++) {
      // Harmonic wave formula: A * sin(k*x - omega*t)
      const y = height / 2 + 50 * Math.sin(x * 0.02 - offset);
      if (x === 0) ctx.moveTo(x, y);
      else ctx.lineTo(x, y);
    }
    
    ctx.stroke();
    offset += 0.05; // Velocity 'c'
    animationId = requestAnimationFrame(draw);
  }
  draw();
});

onUnmounted(() => cancelAnimationFrame(animationId));
</script>

<template><canvas ref="canvas" width="980" height="552" class="bg-canvas"></canvas></template>

<style scoped>
.bg-canvas { position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: -10; pointer-events: none; }
</style>