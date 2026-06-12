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
      <div class="welcome-box-buttons">
        <button class="login-button" @click="$emit('open-login')">вход</button>
        <a href="#" class="guest-link" @click.prevent="$emit('continue-guest')">продолжить как гость</a>
      </div>
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
      duration: 1,
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
      duration: 0.9,
      ease: 'power4.out',
    },
    '-=0.5'
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
      duration: 0.8,
      ease: 'power4.out',
    },
    '-=0.4'
  );
  tl.fromTo(
    '.guest-link',
    {
      opacity: 0,
      y: -40,
    },
    { 
      opacity: 1,
      y: 0,
      duration: 0.7,
      ease: 'power4.out',
    },
    '-=0.3'
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
  background-image: linear-gradient(to bottom, #046A4D, #023522);
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
  gap: 50px;
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
}

.welcome-box-buttons {
  display: flex;
  flex-direction: column;
  gap: 10px;
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

.login-button {
  width: 260px;
  height: 80px;
  font-size: 48px;
  border: none;
  border-radius: 28px;
  cursor: pointer;
  font-weight: 900;
  background-color: #013b2b;
  color: #fff;
  transition: background-color 0.25s ease, box-shadow 0.25s ease;
}

.login-button:hover {
  transform: translateY(-4px) scale(1.03);
  background-color: #012216;
  color: #D1D0D1;
  box-shadow: 0 12px 24px rgba(0,0,0,0.25);
}

.login-button:active {
  transform: scale(0.97);
  background-color: #05442694;
}

.guest-link {
  color: white;
  text-decoration: none;
  font-size: 24px;
  font-weight: 200;
}

.guest-link:hover {
  color: #D1D0D1;
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
    width: 200px;
    height: 60px;
    font-size: 32px;
  }
  
  .guest-link {
    font-size: 18px;
  }
  
  .welcome-box {
    min-height: auto;
    padding: 60px 20px;
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
    width: 180px;
    height: 50px;
    font-size: 24px;
  }
  
  .guest-link {
    font-size: 16px;
  }
  
  .leaf {
    font-size: 16px;
    opacity: 0.08;
  }
}
</style>