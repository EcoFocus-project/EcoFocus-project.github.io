<template>
  <div class="modal-backdrop" @click="$emit('close')">
    <div class="modal" @click.stop>
      <!-- Декоративные элементы -->
      <div class="modal-decor">
        <div class="decor-leaf leaf1">🍃</div>
        <div class="decor-leaf leaf2">🌿</div>
        <div class="decor-leaf leaf3">🍂</div>
        <div class="decor-circle circle1"></div>
        <div class="decor-circle circle2"></div>
      </div>

      <button class="button-close" @click="$emit('close')">✖</button>

      <div class="modal-icon">🔧</div>
      <h1 class="title">ой!</h1>

      <p class="text">данный раздел находится на стадии разработки!</p>
    </div>
  </div>
</template>

<script setup>
import { gsap } from 'gsap'
import { onMounted } from 'vue'

defineEmits(['close'])

const animateModal = () => {
  const tl = gsap.timeline()
  tl.fromTo('.modal-backdrop', { opacity: 0 }, { opacity: 1, duration: 0.25, ease: 'power2.out' })
    .fromTo('.modal', { opacity: 0, scale: 0.85, y: -20 }, { opacity: 1, scale: 1, y: 0, duration: 0.45, ease: 'power4.out' }, '-=0.1')
}

onMounted(() => {
  animateModal()
})
</script>

<style scoped>
/* Анимации */
@keyframes floatLeaf {
  0%, 100% { transform: translateY(0px) rotate(0deg); }
  50% { transform: translateY(-12px) rotate(8deg); }
}

@keyframes floatCircle {
  0%, 100% { transform: translate(0, 0); }
  25% { transform: translate(8px, -8px); }
  50% { transform: translate(-4px, 10px); }
  75% { transform: translate(10px, 4px); }
}

@keyframes gentleFloat {
  0%, 100% { transform: translateY(0px); }
  50% { transform: translateY(-6px); }
}

.modal-backdrop {
  position: fixed;
  inset: 0;
  background: linear-gradient(135deg, rgba(4, 106, 78, 0.85) 0%, rgba(1, 69, 50, 0.9) 100%);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  backdrop-filter: blur(15px);
  padding: 20px;
}

.modal {
  width: 100%;
  max-width: 480px;
  background: linear-gradient(135deg, #046a4e, #014532);
  border-radius: 48px;
  position: relative;
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.3), 0 0 0 1px rgba(50, 180, 144, 0.2);
  padding: 40px 40px 45px;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  border: 1px solid rgba(50, 180, 144, 0.2);
}

/* Декоративные элементы */
.modal-decor {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  pointer-events: none;
  z-index: 0;
  border-radius: 48px;
}

.decor-leaf {
  position: absolute;
  font-size: 24px;
  opacity: 0.1;
  animation: floatLeaf 8s infinite ease-in-out;
}

.leaf1 { top: 10%; left: 5%; animation-delay: 0s; }
.leaf2 { bottom: 15%; right: 8%; animation-delay: 1.5s; font-size: 30px; }
.leaf3 { top: 30%; right: 12%; animation-delay: 0.8s; font-size: 20px; }

.decor-circle {
  position: absolute;
  border-radius: 50%;
  background: rgba(50, 180, 144, 0.05);
  animation: floatCircle 12s infinite;
}

.circle1 { width: 180px; height: 180px; top: -60px; right: -60px; }
.circle2 { width: 140px; height: 140px; bottom: -40px; left: -40px; animation-delay: 1.5s; }

.button-close {
  background: rgba(1, 69, 50, 0.8);
  color: white;
  font-weight: 700;
  font-size: 22px;
  top: 18px;
  right: 18px;
  width: 44px;
  height: 44px;
  border-radius: 50%;
  border: 1px solid rgba(50, 180, 144, 0.3);
  position: absolute;
  cursor: pointer;
  z-index: 10;
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  backdrop-filter: blur(5px);
}

.button-close:hover {
  background: #32b490;
  transform: scale(1.05) rotate(90deg);
  border-color: #55b49a;
}

.modal-icon {
  font-size: 56px;
  margin-bottom: 10px;
  animation: gentleFloat 3s ease-in-out infinite;
  position: relative;
  z-index: 1;
}

.title {
  font-size: 42px;
  font-weight: 900;
  color: white;
  margin: 0 0 15px 0;
  position: relative;
  z-index: 1;
}

.text {
  font-size: 20px;
  font-weight: 400;
  color: rgba(255, 255, 255, 0.85);
  text-align: center;
  margin: 0 0 10px;
  line-height: 1.4;
  position: relative;
  z-index: 1;
}

/* ========== АДАПТИВНОСТЬ ========== */
@media (max-width: 768px) {
  .modal {
    max-width: 420px;
    padding: 35px 30px 40px;
  }
  
  .title {
    font-size: 38px;
    margin-bottom: 12px;
  }
  
  .text {
    font-size: 18px;
  }
  
  .modal-icon {
    font-size: 48px;
  }
  
  .button-close {
    width: 38px;
    height: 38px;
    font-size: 20px;
    top: 14px;
    right: 14px;
  }
  
  .decor-leaf {
    font-size: 20px;
    opacity: 0.08;
  }
  
  .leaf2 {
    font-size: 26px;
  }
}

@media (max-width: 576px) {
  .modal-backdrop {
    padding: 15px;
  }
  
  .modal {
    max-width: 95%;
    padding: 30px 25px 35px;
    border-radius: 35px;
  }
  
  .title {
    font-size: 32px;
    margin-bottom: 10px;
  }
  
  .text {
    font-size: 16px;
  }
  
  .modal-icon {
    font-size: 42px;
    margin-bottom: 8px;
  }
  
  .button-close {
    width: 34px;
    height: 34px;
    font-size: 18px;
    top: 10px;
    right: 10px;
  }
  
  .decor-leaf {
    display: none;
  }
}

@media (max-width: 380px) {
  .modal {
    padding: 25px 20px 30px;
    border-radius: 30px;
  }
  
  .title {
    font-size: 28px;
    margin-bottom: 8px;
  }
  
  .text {
    font-size: 14px;
  }
  
  .modal-icon {
    font-size: 36px;
  }
  
  .button-close {
    width: 30px;
    height: 30px;
    font-size: 16px;
    top: 8px;
    right: 8px;
  }
}
</style>