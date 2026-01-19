<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const canvas = ref(null);
let animationId = null;

const width = 980;
const height = 552;
const centerY = height / 2;

onMounted(() => {
  const ctx = canvas.value.getContext("2d");
  let points = [];
  let t = 0;

  // Configuration
  const limit = centerY;
  const speed = 2; // Horizontal speed
  
  function draw() {
    ctx.fillStyle = "white";
    ctx.fillRect(0, 0, width, height);

    // 1. Draw the "True Solution" (The Limit Line)
    ctx.setLineDash([5, 5]);
    ctx.strokeStyle = "rgba(0, 0, 0, 0.2)";
    ctx.beginPath();
    ctx.moveTo(0, limit);
    ctx.lineTo(width, limit);
    ctx.stroke();
    ctx.setLineDash([]);

    // 2. Generate new converging points
    if (t % 15 === 0) {
      points.push({
        x: 0,
        // Amplitude decreases over time for new points to show "iteration"
        // or we can use a fixed decay based on horizontal position
        age: 0
      });
    }

    // 3. Draw and Update Points
    ctx.lineWidth = 2;
    ctx.lineJoin = "round";
    
    // We draw a path connecting the "approximation" history
    ctx.beginPath();
    ctx.strokeStyle = "rgba(59, 130, 246, 0.6)"; // Blue
    
    for (let i = 0; i < points.length; i++) {
      const p = points[i];
      
      // The Convergence Formula: 
      // y = Limit + Amplitude * e^(-decay * x) * cos(frequency * x)
      const decay = 0.004;
      const freq = 0.05;
      const amplitude = 150;
      
      const y = limit + amplitude * Math.exp(-decay * p.x) * Math.cos(freq * p.x);
      
      if (i === 0) ctx.moveTo(p.x, y);
      else ctx.lineTo(p.x, y);

      // Draw a small dot at the "current" front of the wave
      if (i === points.length - 1) {
        ctx.save();
        ctx.fillStyle = "rgba(59, 130, 246, 1)";
        ctx.beginPath();
        ctx.arc(p.x, y, 4, 0, Math.PI * 2);
        ctx.fill();
        ctx.restore();
      }

      // Move point to the right
      p.x += speed;
    }
    ctx.stroke();

    // Remove points off-screen
    points = points.filter(p => p.x < width);

    t++;
    animationId = requestAnimationFrame(draw);
  }

  draw();
});

onUnmounted(() => {
  if (animationId) cancelAnimationFrame(animationId);
});
</script>

<template>
  <canvas ref="canvas" :width="width" :height="height" class="convergence-canvas"></canvas>
</template>

<style scoped>
.convergence-canvas {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -10;
  pointer-events: none;
}
</style>