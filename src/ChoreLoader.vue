<script setup>
import { ref, computed, onMounted, onUnmounted } from "vue";

const lines = [
  "Herding the dust bunnies…",
  "Waking the chore elves…",
  "Folding that last mystery sock…",
  "Asking the dishwasher nicely…",
  "Fetching chores from the cloud…",
];

const index = ref(0);
const message = computed(() => lines[index.value]);

let timer;

onMounted(() => {
  timer = setInterval(() => {
    index.value = (index.value + 1) % lines.length;
  }, 2400);
});

onUnmounted(() => clearInterval(timer));
</script>

<template>
  <div class="loader" role="status" aria-live="polite" aria-busy="true">
    <div class="scene" aria-hidden="true">
      <span class="bubble b1"></span>
      <span class="bubble b2"></span>
      <span class="bubble b3"></span>
      <span class="bubble b4"></span>
      <span class="bubble b5"></span>

      <div class="washer">
        <div class="washer-top">
          <span class="dial"></span>
          <span class="dial small"></span>
        </div>
        <div class="door">
          <div class="glass">
            <div class="drum">
              <span class="sock coral"></span>
              <span class="sock cream"></span>
              <span class="sock peach"></span>
            </div>
          </div>
          <span class="handle"></span>
        </div>
      </div>

      <div class="floor">
        <div class="bunny one">
          <span class="ear left"></span>
          <span class="ear right"></span>
          <span class="body"></span>
          <span class="eye"></span>
        </div>
        <div class="bunny two">
          <span class="ear left"></span>
          <span class="ear right"></span>
          <span class="body"></span>
          <span class="eye"></span>
        </div>
        <div class="broom">
          <span class="stick"></span>
          <span class="bristles"></span>
        </div>
      </div>
    </div>

    <p class="message" :key="index">{{ message }}</p>
    <p class="hint">The chore closet is waking up — this can take a minute.</p>
  </div>
</template>

<style scoped>
.loader {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 3rem;
  color: #e66b43;
}

.scene {
  position: relative;
  width: 220px;
  height: 250px;
}

.washer {
  position: absolute;
  left: 50%;
  top: 18px;
  transform: translateX(-50%);
  width: 132px;
  height: 158px;
  background: linear-gradient(180deg, #fff 0%, #ffe8de 100%);
  border: 3px solid #ff8a65;
  border-radius: 22px;
  box-shadow: 0 10px 24px rgba(255, 127, 80, 0.22);
}

.washer-top {
  display: flex;
  justify-content: flex-end;
  gap: 6px;
  padding: 10px 12px 0;
}

.dial {
  width: 14px;
  height: 14px;
  border-radius: 50%;
  background: #ff7f50;
  box-shadow: inset 0 -2px 0 rgba(0, 0, 0, 0.08);
  animation: twirl 2.4s linear infinite;
}

.dial.small {
  width: 10px;
  height: 10px;
  background: #ffb199;
  animation-duration: 1.6s;
  animation-direction: reverse;
}

.door {
  position: relative;
  width: 96px;
  height: 96px;
  margin: 12px auto 0;
  border-radius: 50%;
  border: 6px solid #ff9a76;
  background: #f4c4b0;
}

.glass {
  overflow: hidden;
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background: radial-gradient(circle at 35% 30%, #fff8f4 0%, #f7d5c6 55%, #efb9a4 100%);
}

.drum {
  position: relative;
  width: 100%;
  height: 100%;
  animation: tumble 1.8s linear infinite;
}

.sock {
  position: absolute;
  width: 22px;
  height: 28px;
  border-radius: 8px 8px 12px 12px;
  box-shadow: 0 2px 4px rgba(230, 107, 67, 0.2);
}

.sock::after {
  content: "";
  position: absolute;
  top: 4px;
  left: 3px;
  width: 16px;
  height: 6px;
  border-radius: 4px;
  background: rgba(255, 255, 255, 0.55);
}

.sock.coral {
  background: #ff7f50;
  top: 14px;
  left: 28px;
  transform: rotate(-18deg);
}

.sock.cream {
  background: #fff;
  border: 2px solid #ffb199;
  top: 42px;
  left: 48px;
  transform: rotate(28deg);
}

.sock.peach {
  background: #ffc4b0;
  top: 38px;
  left: 12px;
  transform: rotate(-50deg);
}

.handle {
  position: absolute;
  right: -8px;
  top: 42%;
  width: 8px;
  height: 18px;
  border-radius: 4px;
  background: #e66b43;
}

.bubble {
  position: absolute;
  border-radius: 50%;
  border: 2px solid rgba(255, 138, 101, 0.45);
  background: rgba(255, 255, 255, 0.55);
  animation: float 4.2s ease-in infinite;
}

.b1 {
  width: 14px;
  height: 14px;
  left: 18px;
  bottom: 70px;
  animation-delay: 0s;
}

.b2 {
  width: 10px;
  height: 10px;
  right: 22px;
  bottom: 90px;
  animation-delay: 0.8s;
}

.b3 {
  width: 18px;
  height: 18px;
  left: 8px;
  bottom: 120px;
  animation-delay: 1.6s;
}

.b4 {
  width: 8px;
  height: 8px;
  right: 12px;
  bottom: 50px;
  animation-delay: 2.2s;
}

.b5 {
  width: 12px;
  height: 12px;
  left: 40px;
  bottom: 40px;
  animation-delay: 0.4s;
}

.floor {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 8px;
  height: 54px;
}

.bunny {
  position: absolute;
  bottom: 6px;
  width: 28px;
  height: 22px;
}

.bunny .body {
  position: absolute;
  bottom: 0;
  left: 2px;
  width: 24px;
  height: 16px;
  border-radius: 50%;
  background: #d9c4b8;
  box-shadow: 0 2px 0 #c4a899;
}

.bunny .ear {
  position: absolute;
  top: 0;
  width: 7px;
  height: 14px;
  border-radius: 8px;
  background: #d9c4b8;
}

.bunny .ear.left {
  left: 6px;
  transform: rotate(-18deg);
}

.bunny .ear.right {
  left: 14px;
  transform: rotate(18deg);
}

.bunny .eye {
  position: absolute;
  bottom: 8px;
  left: 16px;
  width: 4px;
  height: 4px;
  border-radius: 50%;
  background: #5a4036;
}

.bunny.one {
  left: 8px;
  animation: hop 1.1s ease-in-out infinite;
}

.bunny.two {
  left: 46px;
  animation: hop 1.1s ease-in-out infinite 0.35s;
}

.broom {
  position: absolute;
  right: 10px;
  bottom: 0;
  width: 70px;
  height: 54px;
  transform-origin: 8px 8px;
  animation: sweep 1.4s ease-in-out infinite;
}

.stick {
  position: absolute;
  left: 6px;
  bottom: 18px;
  width: 6px;
  height: 42px;
  border-radius: 4px;
  background: #c47a4a;
  transform: rotate(-38deg);
}

.bristles {
  position: absolute;
  right: 6px;
  bottom: 0;
  width: 34px;
  height: 18px;
  border-radius: 4px 4px 10px 10px;
  background: repeating-linear-gradient(
    90deg,
    #ff7f50 0 4px,
    #ffb199 4px 8px
  );
  transform: rotate(-12deg);
}

.message {
  margin: 0.4rem 0 0;
  font-weight: 700;
  font-size: 1.05rem;
  min-height: 1.5em;
  animation: fade-in 0.35s ease;
}

.hint {
  margin: 0.2rem 0 0;
  color: #b07a68;
  font-size: 0.9rem;
  font-weight: 500;
}

@keyframes tumble {
  to {
    transform: rotate(360deg);
  }
}

@keyframes twirl {
  to {
    transform: rotate(360deg);
  }
}

@keyframes float {
  0% {
    transform: translateY(0) scale(1);
    opacity: 0;
  }
  15% {
    opacity: 0.9;
  }
  100% {
    transform: translateY(-160px) scale(1.2);
    opacity: 0;
  }
}

@keyframes hop {
  0%,
  100% {
    transform: translateY(0);
  }
  40% {
    transform: translateY(-10px);
  }
}

@keyframes sweep {
  0%,
  100% {
    transform: rotate(-8deg);
  }
  50% {
    transform: rotate(14deg);
  }
}

@keyframes fade-in {
  from {
    opacity: 0.4;
    transform: translateY(4px);
  }
  to {
    opacity: 1;
    transform: none;
  }
}

@media (prefers-reduced-motion: reduce) {
  .drum,
  .dial,
  .bubble,
  .bunny,
  .broom,
  .message {
    animation: none;
  }
}
</style>
