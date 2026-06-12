<template>
  <div class="modal-backdrop" @click="$emit('close')">
    <div class="modal" :class="`modal-${mode}`" @click.stop>
      <button class="button-close" @click="$emit('close')">✖</button>

      <!-- ТОЛЬКО ДЕКОР - добавил листья и круги -->
      <div class="modal-decor">
        <div class="decor-leaf leaf1">🍃</div>
        <div class="decor-leaf leaf2">🌿</div>
        <div class="decor-leaf leaf3">🍂</div>
        <div class="decor-leaf leaf4">🌱</div>
        <div class="decor-circle circle1"></div>
        <div class="decor-circle circle2"></div>
      </div>

      <LoginForm
        v-if="mode === 'login'"
        @switch-mode="$emit('switch-mode', $event)"
        @success-login="$emit('success-login')"
      />

      <RegisterForm
        v-else-if="mode === 'register'"
        @switch-mode="$emit('switch-mode', $event)"
        @success-login="$emit('success-login')"
      />
    </div>
  </div>
</template>

<script setup>
import { gsap } from 'gsap'
import { onMounted, onUnmounted } from 'vue'
import LoginForm from './LoginForm.vue'
import RegisterForm from './RegisterForm.vue'

defineProps({
  mode: {
    type: String,
    required: true,
  },
})

defineEmits(['close', 'switch-mode', 'success-login'])

const animateModal = () => {
  const tl = gsap.timeline()
  tl.fromTo('.modal-backdrop', { opacity: 0 }, { opacity: 1, duration: 0.25, ease: 'power2.out' })
    .fromTo('.modal', { opacity: 0, scale: 0.85, y: -20 }, { opacity: 1, scale: 1, y: 0, duration: 0.45, ease: 'power4.out' }, '-=0.1')
}

const lockScroll = () => {
  document.body.style.overflow = 'hidden'
  document.body.style.position = 'fixed'
  document.body.style.width = '100%'
}

const unlockScroll = () => {
  document.body.style.overflow = ''
  document.body.style.position = ''
  document.body.style.width = ''
}

onMounted(() => {
  animateModal()
  lockScroll()
})

onUnmounted(() => {
  unlockScroll()
})
</script>

<style scoped>
/* Твои оригинальные стили полностью сохранены */
.modal-backdrop {
  position: fixed;
  inset: 0;
  background-color: rgba(0, 0, 0, 0.45);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  padding: 16px;
  box-sizing: border-box;
}

.modal {
  width: 100%;
  max-width: 580px;
  background-color: #046A4D;
  border-radius: 30px;
  display: flex;
  flex-direction: column;
  position: relative;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.25);
  padding: 24px 32px 28px;
  box-sizing: border-box;
  max-height: calc(100vh - 64px);
  overflow-y: auto;
}

.modal-login,
.modal-register {
  min-height: auto;
  height: auto;
  padding-bottom: 0;
}

.button-close {
  background-color: #014532;
  color: white;
  font-weight: 700;
  font-size: 25px;
  top: 18px;
  right: 18px;
  width: 49px;
  height: 49px;
  border-radius: 50%;
  border: none;
  position: absolute;
  cursor: pointer;
  z-index: 10;
}

/* === ДОБАВЛЕН ТОЛЬКО ДЕКОР (листья и круги) === */
.modal-decor {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  pointer-events: none;
  z-index: 0;
  border-radius: 30px;
}

@keyframes decorFloatLeaf {
  0%, 100% { transform: translateY(0px) rotate(0deg); }
  50% { transform: translateY(-12px) rotate(8deg); }
}

@keyframes decorFloatCircle {
  0%, 100% { transform: translate(0, 0); }
  25% { transform: translate(8px, -8px); }
  50% { transform: translate(-4px, 10px); }
  75% { transform: translate(10px, 4px); }
}

.decor-leaf {
  position: absolute;
  font-size: 24px;
  opacity: 0.12;
  animation: decorFloatLeaf 8s infinite ease-in-out;
}

.leaf1 { top: 10%; left: 5%; animation-delay: 0s; }
.leaf2 { bottom: 15%; right: 8%; animation-delay: 1.2s; font-size: 30px; }
.leaf3 { top: 25%; right: 12%; animation-delay: 0.6s; font-size: 20px; }
.leaf4 { bottom: 20%; left: 10%; animation-delay: 2s; font-size: 28px; }

.decor-circle {
  position: absolute;
  border-radius: 50%;
  background: rgba(50, 180, 144, 0.05);
  animation: decorFloatCircle 12s infinite ease-in-out;
}

.circle1 {
  width: 180px;
  height: 180px;
  top: -60px;
  right: -60px;
}

.circle2 {
  width: 140px;
  height: 140px;
  bottom: -40px;
  left: -40px;
  animation-delay: 1.5s;
}
/* === КОНЕЦ ДОБАВЛЕННОГО ДЕКОРА === */

/* Твоя адаптивность полностью сохранена */
@media (max-width: 768px) {
  .modal {
    padding: 20px 24px 24px;
    border-radius: 24px;
  }
}

@media (max-width: 480px) {
  .modal {
    padding: 16px 20px 20px;
    border-radius: 16px;
  }
  .button-close {
    top: 8px;
    right: 8px;
    width: 36px;
    height: 36px;
    font-size: 18px;
  }
  .decor-leaf, .decor-circle {
    display: none;
  }
}

@media (max-height: 500px) {
  .modal {
    max-height: calc(100vh - 32px);
    padding: 12px 16px 16px;
  }
}
</style>