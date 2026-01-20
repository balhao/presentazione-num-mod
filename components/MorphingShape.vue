<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const canvas = ref(null);
let animationId = null;

const width = 980;
const height = 552;

// Shape Configuration
const settings = {
  segments: 24,       // Smoothness of the circle
  rings: 8,           // Vertical resolution
  baseRadius: 80,
  height: 180,
  rotationSpeed: 0.015,
  morphSpeed: 0.02,
  focalLength: 400,
  hue: 212,
  saturation: 90,
  lightness: 65,
};

let angleX = 0, angleY = 0;
let morphTime = 0;

function project(x, y, z, centerX, centerY) {
  // Rotation
  const cosX = Math.cos(angleX), sinX = Math.sin(angleX);
  const cosY = Math.cos(angleY), sinY = Math.sin(angleY);

  let y1 = y * cosX - z * sinX;
  let z1 = y * sinX + z * cosX;
  let x2 = x * cosY + z1 * sinY;
  let z2 = -x * sinY + z1 * cosY;

  // Perspective
  const scale = settings.focalLength / (settings.focalLength + z2);
  return {
    x: centerX + x2 * scale,
    y: centerY + y1 * scale
  };
}

onMounted(() => {
  const ctx = canvas.value.getContext("2d");
  const pixelRatio = 2;
  canvas.value.width = width * pixelRatio;
  canvas.value.height = height * pixelRatio;
  ctx.setTransform(pixelRatio, 0, 0, pixelRatio, 0, 0);

  function draw() {
    ctx.fillStyle = "rgba(255, 255, 255, 1)";
    ctx.fillRect(0, 0, width, height);

    // Morphing Factor: 0 = Cone, 1 = Cylinder
    const morphFactor = (Math.sin(morphTime) + 1) / 2;
    
    // Movement: Floating center
    const centerX = width / 2 + Math.cos(morphTime * 0.5) * 100;
    const centerY = height / 2 + Math.sin(morphTime * 0.3) * 50;

    ctx.strokeStyle = `hsla(${settings.hue}, ${settings.saturation}%, ${settings.lightness}%, 0.7)`;
    ctx.lineWidth = 1.5;
    ctx.beginPath();

    for (let r = 0; r <= settings.rings; r++) {
      const v = r / settings.rings; // 0 at top, 1 at bottom
      const y = -settings.height / 2 + v * settings.height;
      
      // The Morphing Logic: Top radius varies, bottom stays fixed
      const currentRadius = settings.baseRadius * (morphFactor + (1 - morphFactor) * v);

      for (let s = 0; s <= settings.segments; s++) {
        const theta = (s / settings.segments) * Math.PI * 2;
        const x = currentRadius * Math.cos(theta);
        const z = currentRadius * Math.sin(theta);

        const p = project(x, y, z, centerX, centerY);
        if (s === 0) ctx.moveTo(p.x, p.y);
        else ctx.lineTo(p.x, p.y);

        // Vertical Edges
        if (r < settings.rings) {
            const nextV = (r + 1) / settings.rings;
            const nextY = -settings.height / 2 + nextV * settings.height;
            const nextRadius = settings.baseRadius * (morphFactor + (1 - morphFactor) * nextV);
            const nextX = nextRadius * Math.cos(theta);
            const nextZ = nextRadius * Math.sin(theta);
            const pNext = project(nextX, nextY, nextZ, centerX, centerY);
            
            ctx.moveTo(p.x, p.y);
            ctx.lineTo(pNext.x, pNext.y);
            ctx.moveTo(p.x, p.y); // Back to current for loop
        }
      }
    }
    ctx.stroke();

    angleX += settings.rotationSpeed;
    angleY += settings.rotationSpeed * 0.6;
    morphTime += settings.morphSpeed;
    animationId = requestAnimationFrame(draw);
  }
  draw();
});

onUnmounted(() => cancelAnimationFrame(animationId));
</script>

<template>
  <canvas ref="canvas" class="morph-canvas"></canvas>
</template>

<style scoped>
.morph-canvas {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -10;
  pointer-events: none;
}
</style>