<template>
  <div class="page">
    <div class="welcome-box">
      <div class=".welcome-box-text">
        <h1 class="title">добро пожаловать в GreenMind</h1>
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
import { ref , onMounted } from 'vue';
const title = ref(null);
const animateInfo = () => {
  const tl = gsap.timeline();
  tl.fromTo(
    '.title',
    {
      opacity: 0,
      y: -150,
    },
    { opacity: 1,
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
    { opacity: 1,
      y: 0,
      duration: 0.9,
      ease: 'power4.out',
    },
    '-=0.5'
  )
      tl.fromTo(
    '.login-button',
    {
      opacity: 0,
      y: -60,
    },
    { opacity: 1,
      y: 0,
      duration: 0.8,
      ease: 'power4.out',
    },
    '-=0.4'
  )
      tl.fromTo(
    '.guest-link',
    {
      opacity: 0,
      y: -40,
    },
    { opacity: 1,
      y: 0,
      duration: 0.7,
      ease: 'power4.out',
    },
    '-=0.3'
  )
};
onMounted( () => {
  animateInfo();
}
)

defineEmits(['open-login', 'continue-guest'])
</script>

<style scoped>
.page {
  min-height: 100vh;
  background-image: linear-gradient(to bottom, #046A4D, #023522);
  display: flex;
  justify-content: center;
  align-items: center;
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
</style>