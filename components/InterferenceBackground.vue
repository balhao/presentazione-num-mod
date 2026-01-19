<script setup>
import { ref, onMounted, onUnmounted } from "vue";
const canvas = ref(null);
let animationId = null;

// Drawing dimensions
const width = 980;
const height = 552;
// Internal resolution for performance (upscaled later)
const resX = 245;
const resY = 138;

onMounted(() => {
  const canvasEl = canvas.value;
  const ctx = canvasEl.getContext("2d");
  let t = 0;

  function draw() {
    // Clear the main canvas to white
    ctx.fillStyle = "rgba(255, 255, 255, 1)";
    ctx.fillRect(0, 0, width, height);

    const imageData = ctx.createImageData(resX, resY);
    const data = imageData.data;

    for (let j = 0; j < resY; j++) {
      for (let i = 0; i < resX; i++) {
        // Distance from two point sources
        const d1 = Math.sqrt(Math.pow(i - resX * 0.3, 2) + Math.pow(j - resY * 0.5, 2));
        const d2 = Math.sqrt(Math.pow(i - resX * 0.7, 2) + Math.pow(j - resY * 0.5, 2));
        
        // Superposition of two waves
        const val = Math.sin(d1 * 0.5 - t) + Math.sin(d2 * 0.5 - t);
        
        // Map the wave intensity (-2 to 2) to an opacity (0 to 100)
        // We keep RGB constant at a light gray (e.g., 200)
        const alpha = (val + 2) * 15; // Subtle visibility
        const index = (i + j * resX) * 4;

        data[index]     = 180;   // R (Light Gray)
        data[index + 1] = 180;   // G
        data[index + 2] = 180;   // B
        data[index + 3] = alpha; // Alpha channel creates the pattern
      }
    }

    // Create an offscreen buffer to scale up the image smoothly
    const tempCanvas = document.createElement('canvas');
    tempCanvas.width = resX;
    tempCanvas.height = resY;
    tempCanvas.getContext('2d').putImageData(imageData, 0, 0);

    // Draw the low-res pattern scaled up to the full slide size
    ctx.globalAlpha = 0.6; // Overall transparency
    ctx.drawImage(tempCanvas, 0, 0, resX, resY, 0, 0, width, height);
    ctx.globalAlpha = 1.0;

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
  <canvas ref="canvas" :width="width" :height="height" class="interference-canvas"></canvas>
</template>

<style scoped>
.interference-canvas {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -10;
  pointer-events: none;
  background-color: white;
}
</style>