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

      <div class="modal-icon">⚠️</div>
      <h2 class="modal-title">вы уверены?</h2>

      <p class="modal-text">
        данное действие нельзя отменить.
        <br />
        вы потеряете весь прогресс!
      </p>

      <div class="buttons-row">
        <button class="delete-button" @click="$emit('confirm-deactivate')">
          <span class="button-text">удалить аккаунт</span>
          <span class="button-icon">→</span>
        </button>

        <button class="cancel-button" @click="$emit('close')">
          <span class="button-text">отмена</span>
          <span class="button-icon">←</span>
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { gsap } from 'gsap'
import { onMounted } from 'vue'

defineEmits(['close', 'confirm-deactivate'])

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

@keyframes pulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.1); }
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
  max-width: 520px;
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
  animation: pulse 2s ease-in-out infinite;
  position: relative;
  z-index: 1;
}

.modal-title {
  font-size: 42px;
  font-weight: 900;
  color: white;
  margin: 0 0 15px 0;
  position: relative;
  z-index: 1;
}

.modal-text {
  font-size: 20px;
  font-weight: 400;
  color: rgba(255, 255, 255, 0.85);
  text-align: center;
  margin: 0 0 30px;
  line-height: 1.4;
  position: relative;
  z-index: 1;
}

.buttons-row {
  display: flex;
  gap: 14px;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  position: relative;
  z-index: 1;
}

/* Кнопки */
.delete-button,
.cancel-button {
  min-width: 180px;
  height: 54px;
  border: none;
  border-radius: 50px;
  font-size: 18px;
  font-weight: 900;
  color: white;
  cursor: pointer;
  transition: all 0.25s ease;
  position: relative;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0 24px;
}

.button-text {
  transition: transform 0.2s ease;
}

.button-icon {
  position: absolute;
  font-size: 18px;
  opacity: 0;
  transform: translateX(-10px);
  transition: all 0.2s ease;
}

.delete-button .button-icon {
  right: 20px;
}

.cancel-button .button-icon {
  left: 20px;
}

.delete-button::before,
.cancel-button::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: left 0.4s ease;
}

.delete-button:hover::before,
.cancel-button:hover::before {
  left: 100%;
}

.delete-button:hover,
.cancel-button:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.25);
}

.delete-button:hover .button-text,
.cancel-button:hover .button-text {
  transform: translateX(-5px);
}

.delete-button:hover .button-icon,
.cancel-button:hover .button-icon {
  opacity: 1;
  transform: translateX(0);
}

.cancel-button:hover .button-text {
  transform: translateX(5px);
}

.delete-button:active,
.cancel-button:active {
  transform: scale(0.97);
}

.delete-button {
  background: linear-gradient(135deg, #c0392b, #a93226);
}

.delete-button:hover {
  background: linear-gradient(135deg, #e74c3c, #c0392b);
}

.cancel-button {
  background: rgba(1, 59, 43, 0.9);
  border: 1px solid rgba(50, 180, 144, 0.3);
}

.cancel-button:hover {
  background: rgba(1, 45, 33, 0.95);
  border-color: #32b490;
}

/* ========== АДАПТИВНОСТЬ ========== */
@media (max-width: 768px) {
  .modal {
    max-width: 450px;
    padding: 35px 30px 40px;
  }
  
  .modal-title {
    font-size: 36px;
    margin-bottom: 12px;
  }
  
  .modal-text {
    font-size: 18px;
    margin-bottom: 25px;
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
  
  .delete-button,
  .cancel-button {
    min-width: 160px;
    height: 50px;
    font-size: 16px;
    padding: 0 20px;
  }
  
  .delete-button .button-icon {
    right: 18px;
  }
  
  .cancel-button .button-icon {
    left: 18px;
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
  
  .modal-title {
    font-size: 32px;
    margin-bottom: 10px;
  }
  
  .modal-text {
    font-size: 16px;
    margin-bottom: 22px;
  }
  
  .modal-text br {
    display: none;
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
  
  .buttons-row {
    gap: 10px;
    flex-direction: column;
    width: 100%;
  }
  
  .delete-button,
  .cancel-button {
    min-width: 0;
    width: 100%;
    max-width: 220px;
    height: 46px;
    font-size: 15px;
    padding: 0 16px;
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
  
  .modal-title {
    font-size: 28px;
    margin-bottom: 8px;
  }
  
  .modal-text {
    font-size: 14px;
    margin-bottom: 18px;
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
  
  .delete-button,
  .cancel-button {
    height: 42px;
    font-size: 14px;
  }
}
</style>