<template>
  <div class="page">
    <!-- Декоративные элементы фона -->
    <div class="bg-decoration">
      <div class="leaf leaf1">🍃</div>
      <div class="leaf leaf2">🌿</div>
      <div class="leaf leaf3">🍂</div>
      <div class="leaf leaf4">🌱</div>
      <div class="leaf leaf5">🍃</div>
      <div class="leaf leaf6">🌿</div>
      <div class="circle circle1"></div>
      <div class="circle circle2"></div>
      <div class="circle circle3"></div>
    </div>

    <div class="welcome-box">
      <div class="welcome-box-text">
        <h1 class="title">добро пожаловать в ЭкоФокус</h1>
        <p class="subtitle">развивай экологическое мышление шаг за шагом</p>
      </div>
      
      <button class="login-button" @click="$emit('open-login')">
        <span class="login-text">вход</span>
        <span class="login-arrow">→</span>
      </button>
      
      <button class="guest-button" @click="$emit('continue-guest')">
        <span class="guest-text">продолжить как гость</span>
        <span class="guest-arrow">→</span>
      </button>
    </div>
  </div>
</template>

<script setup>
import { gsap } from 'gsap'
import { onMounted } from 'vue';

const animateInfo = () => {
  const tl = gsap.timeline();
  tl.fromTo(
    '.title',
    {
      opacity: 0,
      y: -150,
    },
    { 
      opacity: 1,
      y: 0,
      duration: 0.8,
      ease: 'power4.out',
    }
  );
  tl.fromTo(
    '.subtitle',
    {
      opacity: 0,
      y: -80,
    },
    { 
      opacity: 1,
      y: 0,
      duration: 0.7,
      ease: 'power4.out',
    },
    '-=0.4'
  );
  tl.fromTo(
    '.login-button',
    {
      opacity: 0,
      y: -60,
    },
    { 
      opacity: 1,
      y: 0,
      duration: 0.5,
      ease: 'power4.out',
    },
    '-=0.3'
  );
  tl.fromTo(
    '.guest-button',
    {
      opacity: 0,
      y: -40,
    },
    { 
      opacity: 1,
      y: 0,
      duration: 0.5,
      ease: 'power4.out',
    },
    '-=0.2'
  );
};

onMounted(() => {
  animateInfo();
});

defineEmits(['open-login', 'continue-guest']);
</script>

<style scoped>
.page {
  min-height: 100vh;
  background-image: linear-gradient(to bottom, #046a4e, #014532);
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  overflow: hidden;
}

/* Декоративные элементы фона */
.bg-decoration {
  position: absolute;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 0;
}

.leaf {
  position: absolute;
  font-size: 30px;
  opacity: 0.15;
  animation: floatLeaf 8s infinite ease-in-out;
}

.leaf1 {
  top: 10%;
  left: 5%;
  animation-delay: 0s;
}

.leaf2 {
  top: 20%;
  right: 8%;
  animation-delay: 1s;
  font-size: 40px;
}

.leaf3 {
  bottom: 15%;
  left: 10%;
  animation-delay: 2s;
  font-size: 25px;
}

.leaf4 {
  bottom: 25%;
  right: 15%;
  animation-delay: 0.5s;
  font-size: 35px;
}

.leaf5 {
  top: 40%;
  left: 15%;
  animation-delay: 1.5s;
  font-size: 28px;
}

.leaf6 {
  bottom: 40%;
  right: 20%;
  animation-delay: 2.5s;
  font-size: 32px;
}

@keyframes floatLeaf {
  0%, 100% {
    transform: translateY(0px) rotate(0deg);
  }
  50% {
    transform: translateY(-20px) rotate(10deg);
  }
}

.circle {
  position: absolute;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.03);
  pointer-events: none;
}

.circle1 {
  width: 300px;
  height: 300px;
  top: -100px;
  right: -100px;
}

.circle2 {
  width: 500px;
  height: 500px;
  bottom: -200px;
  left: -150px;
}

.circle3 {
  width: 200px;
  height: 200px;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.welcome-box {
  min-height: 960px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 20px;
  padding: 40px;
  text-align: center;
  user-select: none;
  position: relative;
  z-index: 1;
}

.welcome-box-text {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-bottom: 20px;
}

.title {
  font-size: 64px;
  margin: 0;
  font-weight: 900;
  color: #fff;
}

.subtitle {
  font-size: 40px;
  margin: 0;
  color: #fff;
  font-weight: 400;
}

/* Кнопка входа */
.login-button {
  width: 300px;
  height: 65px;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  background: linear-gradient(135deg, #32b490 0%, #046a4e 100%);
  color: #fff;
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
  font-size: 35px;
  font-weight: 900;
  margin-top: 20px;
}

.login-text {
  transition: transform 0.2s ease;
}

.login-arrow {
  position: absolute;
  font-size: 24px;
  opacity: 0;
  transform: translateX(-10px);
  transition: all 0.2s ease;
  right: 30px;
}

.login-button::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
  transition: left 0.4s ease;
}

.login-button:hover::before {
  left: 100%;
}

.login-button:hover {
  transform: translateY(-3px) scale(1.02);
  background: linear-gradient(135deg, #55b49a 0%, #32b490 100%);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
}

.login-button:hover .login-text {
  transform: translateX(-15px);
}

.login-button:hover .login-arrow {
  opacity: 1;
  transform: translateX(0);
}

/* Кнопка гостя */
.guest-button {
  background: transparent;
  border: none;
  cursor: pointer;
  color: rgba(255, 255, 255, 0.85);
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  padding: 6px 0 12px 0;
  font-size: 20px;
  font-weight: 400;
  width: auto;
}

.guest-text {
  transition: transform 0.2s ease;
}

.guest-arrow {
  position: absolute;
  font-size: 22px;
  opacity: 0;
  transform: translateX(-10px);
  transition: all 0.2s ease;
  right: -30px;
}

.guest-button::before {
  content: '';
  position: absolute;
  bottom: 0;
  left: 50%;
  width: 0;
  height: 2px;
  background: linear-gradient(90deg, #32b490, #55b49a);
  transition: all 0.2s ease;
  transform: translateX(-50%);
}

.guest-button:hover {
  color: white;
}

.guest-button:hover .guest-text {
  transform: translateX(-10px);
}

.guest-button:hover .guest-arrow {
  opacity: 1;
  transform: translateX(0);
}

.guest-button:hover::before {
  width: calc(100% + 20px);
}

.guest-button:active {
  transform: scale(0.98);
}

/* Адаптивность */
@media (max-width: 768px) {
  .title {
    font-size: 36px;
  }
  
  .subtitle {
    font-size: 24px;
  }
  
  .login-button {
    width: 220px;
    height: 55px;
    font-size: 24px;
    margin-top: 15px;
  }
  
  .login-arrow {
    font-size: 20px;
    right: 25px;
  }
  
  .guest-button {
    font-size: 18px;
    margin-top: 3px;
  }
  
  .guest-arrow {
    font-size: 18px;
    right: -28px;
  }
  
  .welcome-box {
    min-height: auto;
    padding: 60px 20px;
  }
  
  .welcome-box-text {
    margin-bottom: 15px;
  }
  
  .leaf {
    font-size: 20px;
    opacity: 0.1;
  }
  
  .leaf2, .leaf4, .leaf6 {
    font-size: 25px;
  }
}

@media (max-width: 480px) {
  .title {
    font-size: 28px;
  }
  
  .subtitle {
    font-size: 18px;
  }
  
  .login-button {
    width: 200px;
    height: 50px;
    font-size: 22px;
    margin-top: 10px;
  }
  
  .login-arrow {
    font-size: 18px;
    right: 20px;
  }
  
  .guest-button {
    font-size: 16px;
  }
  
  .guest-arrow {
    font-size: 16px;
    right: -25px;
  }
  
  .leaf {
    font-size: 16px;
    opacity: 0.08;
  }
}
</style>