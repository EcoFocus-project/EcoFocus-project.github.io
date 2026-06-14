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

      <div class="modal-icon">🔒</div>
      <h1 class="title">упс..</h1>

      <p class="text">
        для этого вам необходимо
        <br />
        иметь аккаунт.
      </p>
      
      <div class="buttons-block">
        <button class="button-submit button-login" @click="$emit('open-login')">
          <span class="button-text">войти</span>
          <span class="button-icon">→</span>
        </button>
        <button class="button-submit button-register" @click="$emit('open-register')">
          <span class="button-text">зарегистрироваться</span>
          <span class="button-icon">→</span>
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { gsap } from 'gsap'
import { onMounted } from 'vue'

defineEmits(['close', 'open-login', 'open-register'])

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
  max-width: 580px; 
  background: linear-gradient(135deg, #046a4e, #014532);
  border-radius: 48px;
  display: flex;
  flex-direction: column;
  position: relative;
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.3), 0 0 0 1px rgba(50, 180, 144, 0.2);
  text-align: center;
  border: 1px solid rgba(50, 180, 144, 0.2);
  padding: 26px 34px 30px;
  gap: 12px;
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
  animation: gentleFloat 3s ease-in-out infinite;
  position: relative;
  z-index: 1;
}

.title {
  font-size: 42px;
  font-weight: 900;
  text-align: center;
  color: white;
  margin: 0;
  position: relative;
  z-index: 1;
}

.text {
  font-size: 20px;
  font-weight: 400;
  text-align: center;
  color: rgba(255, 255, 255, 0.85);
  margin: 0 0 5px 0;
  line-height: 1.4;
  position: relative;
  z-index: 1;
}

.buttons-block {
  display: flex;
  flex-direction: column;
  align-items: center; /* Центрируем по горизонтали */
  gap: 12px;
  width: 100%;
  position: relative;
  z-index: 1;
  margin-top: auto;
}


/* Кнопки */
.button-submit {
  width: 100%;
  max-width: 300px; /* Немного уменьшили, чтобы точно влезло */
  height: 54px;
  font-size: 20px;
  font-weight: 900;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  transition: all 0.25s ease;
  position: relative;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}


.button-text {
  transition: transform 0.2s ease;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}

.button-icon {
  position: absolute;
  font-size: 20px;
  opacity: 0;
  transform: translateX(-15px);
  transition: all 0.2s ease;
  right: 25px;
}

.button-submit::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: left 0.4s ease;
}

.button-submit:hover::before {
  left: 100%;
}

.button-submit:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.25);
}

.button-submit:hover .button-text {
  transform: translateX(calc(-50% - 12px));
}

.button-submit:hover .button-icon {
  opacity: 1;
  transform: translateX(0);
}

.button-submit:active {
  transform: scale(0.97);
}

.button-login {
  background: linear-gradient(135deg, #32b490 0%, #046a4e 100%);
  color: white;
}

.button-login:hover {
  background: linear-gradient(135deg, #55b49a 0%, #32b490 100%);
}

.button-register {
  background: rgba(1, 59, 43, 0.9);
  border: 1px solid rgba(50, 180, 144, 0.3);
  color: white;
}

.button-register:hover {
  background: rgba(1, 45, 33, 0.95);
  border-color: #32b490;
}

/* ========== АДАПТИВНОСТЬ ========== */
@media (max-width: 768px) {
  .modal {
    max-width: 520px;
    padding: 35px 30px 40px;
    border-radius: 40px;
  }
  
  .title {
    font-size: 38px;
  }
  
  .text {
    font-size: 18px;
    margin-bottom: 25px;
  }
  
  .modal-icon {
    font-size: 48px;
  }
  
  .button-close {
    width: 40px;
    height: 40px;
    font-size: 20px;
    top: 14px;
    right: 14px;
  }
  
  .button-submit {
    max-width: 280px; /* Было 260 px */
    height: 52px; /* Оставляем близким к десктопу */
    font-size: 19px; /* Чуть крупнее */
  }

  .buttons-block {
    padding: 0 15px; /* Горизонтальные отступы для компактности */
  }
  
  .button-icon {
    font-size: 19px;
    right: 24px;
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
  }
  
  .text {
    font-size: 16px;
    margin-bottom: 22px;
  }
  
  .modal-icon {
    font-size: 42px;
  }
  
  .button-close {
    width: 36px;
    height: 36px;
    font-size: 18px;
    top: 12px;
    right: 12px;
  }
  
  .button-submit {
    max-width: 260px; /* Было 240 px */
    height: 48px; /* Не опускаем слишком низко */
    font-size: 17px; /* Сохраняем читаемость */
  }
  
  .button-icon {
    font-size: 17px;
    right: 22px;
  }
  
  .buttons-block {
    gap: 12px; /* Немного увеличиваем отступ между кнопками */
    padding: 0 10px; /* Отступы по краям */
  }
  
  .decor-leaf {
    display: none;
  }
}

@media (max-width: 380px) {
  .modal {
    padding: 25px 15px 30px; /* Уменьшаем боковые отступы модалки */
  }
  
  .title {
    font-size: 28px;
  }
  
  .text {
    font-size: 14px;
    margin-bottom: 18px;
  }
  
  .modal-icon {
    font-size: 36px;
  }
  
  .button-close {
    width: 32px;
    height: 32px;
    font-size: 16px;
    top: 10px;
    right: 10px;
  }
  
  .button-submit {
    max-width: 240px; /* Было 220 px */
    height: 44px; /* Минимум для удобного тапа */
    font-size: 15px; /* Допустимый минимум */
  }
  
  .button-icon {
    font-size: 15px;
    right: 20px;
  }

  .buttons-block {
    padding: 0 8px; /* Компактные отступы */
  }
}
</style>