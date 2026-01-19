<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const canvas = ref(null);
let animationId = null;

const width = 980;
const height = 552;
const gridStep = 20; // Distance between grid lines

onMounted(() => {
  const ctx = canvas.value.getContext("2d");
  let t = 0;

  // Sources of the ripples
  const sources = [
    { x: width * 0.3, y: height * 0.4, phase: 0 },
    { x: width * 0.7, y: height * 0.6, phase: Math.PI }
  ];

  function draw() {
    ctx.fillStyle = "white";
    ctx.fillRect(0, 0, width, height);

    ctx.strokeStyle = "rgba(100, 110, 140, 0.2)";
    ctx.lineWidth = 1;

    // Draw horizontal grid lines with 3D perspective warp
    for (let y = 0; y < height; y += gridStep) {
      ctx.beginPath();
      for (let x = 0; x < width; x += 5) {
        let z = 0;
        
        // Calculate interference from all sources
        sources.forEach(s => {
          const dist = Math.sqrt(Math.pow(x - s.x, 2) + Math.pow(y - s.y, 2));
          // Wave equation: sin(k*r - w*t) / damping
          z += Math.sin(dist * 0.05 - t) * Math.exp(-dist * 0.002) * 15;
        });

        // Perspective mapping: offset y by z to simulate "height"
        if (x === 0) ctx.moveTo(x, y + z);
        else ctx.lineTo(x, y + z);
      }
      ctx.stroke();
    }

    // Draw vertical grid lines
    for (let x = 0; x < width; x += gridStep) {
      ctx.beginPath();
      for (let y = 0; y < height; y += 5) {
        let z = 0;
        sources.forEach(s => {
          const dist = Math.sqrt(Math.pow(x - s.x, 2) + Math.pow(y - s.y, 2));
          z += Math.sin(dist * 0.05 - t) * Math.exp(-dist * 0.002) * 15;
        });

        if (y === 0) ctx.moveTo(x, y + z);
        else ctx.lineTo(x, y + z);
      }
      ctx.stroke();
    }

    t += 0.08;
    animationId = requestAnimationFrame(draw);
  }

  draw();
});

onUnmounted(() => {
  if (animationId) cancelAnimationFrame(animationId);
});
</script>

<template>
  <canvas ref="canvas" :width="width" :height="height" class="gravity-3d-canvas"></canvas>
</template>

<style scoped>
.gravity-3d-canvas {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -10;
  pointer-events: none;
}
</style>