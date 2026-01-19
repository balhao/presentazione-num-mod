<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const canvas = ref(null);
let animationId = null;

const width = 980;
const height = 552;

// Configuration
const waveFrequency = 40; // How often a new drop falls (lower = more frequent)
const expansionSpeed = 1.5;
const initialRadius = 5;
const maxRadius = 600;

onMounted(() => {
  const ctx = canvas.value.getContext("2d");
  let waves = [];
  let frame = 0;

  function createWave() {
    waves.push({
      x: Math.random() * width,
      y: Math.random() * height,
      r: initialRadius,
      opacity: 0.6
    });
  }

  function draw() {
    // Clear canvas to pure white
    ctx.fillStyle = "white";
    ctx.fillRect(0, 0, width, height);

    if (frame % waveFrequency === 0) createWave();

    waves.forEach((wave, index) => {
      // Draw the circular wave
      ctx.beginPath();
      ctx.arc(wave.x, wave.y, wave.r, 0, Math.PI * 2);
      ctx.strokeStyle = `rgba(100, 100, 100, ${wave.opacity})`;
      ctx.lineWidth = 2;
      ctx.stroke();

      // Update wave properties
      wave.r += expansionSpeed;
      // Linear fade out as it grows
      wave.opacity = 0.6 * (1 - wave.r / maxRadius);

      // Remove waves that have faded out or grown too large
      if (wave.opacity <= 0 || wave.r >= maxRadius) {
        waves.splice(index, 1);
      }
    });

    frame++;
    animationId = requestAnimationFrame(draw);
  }

  draw();
});

onUnmounted(() => {
  if (animationId) cancelAnimationFrame(animationId);
});
</script>

<template>
  <canvas ref="canvas" :width="width" :height="height" class="gravity-canvas"></canvas>
</template>

<style scoped>
.gravity-canvas {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -10;
  pointer-events: none;
}
</style>