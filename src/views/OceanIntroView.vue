<template>
  <div class="page">
    <div class="header">
      <img :src="logoImg" alt="логотип" class="logo" />

      <div class="header-buttons">
        <button class="nav-button" @click="handleProfileClick">
          <img :src="profileImg" alt="профиль" class="nav-icon" />
          <div class="nav-text">
            <p>мой профиль</p>
          </div>
        </button>

        <button class="nav-button" @click="handleAchievementsClick">
          <img :src="achievementsImg" alt="достижение" class="nav-icon" />
          <div class="nav-text">
            <p>достижения</p>
          </div>
        </button>

        <button class="nav-button" @click="handleQuestClick">
          <img :src="dailyImg" alt="ежедневное задание" class="nav-icon" />
          <div class="nav-text">
            <p>ежедневное задание</p>
          </div>
        </button>
      </div>
    </div>

    <div class="content">
      <div class="intro-card">
        <div class="intro-left">
          <p class="intro-label">достижение</p>
          <h1 class="intro-title">друг океанов</h1>
          <img :src="oceanImg" alt="логотип" class="intro-logo" />

          <p class="intro-description">
            Погрузись в глубины океана и познакомься с его удивительными
            обитателями. Но океан нуждается в помощи! Выполняй задания, решай
            экологические задачи и помоги сохранить чистоту морей.
          </p>
        </div>

        <div class="intro-divider"></div>

        <div class="intro-right">
          <button class="level-card current-level" @click="$emit('open-level-one')">
            <div class="level-text-block">
              <p class="level-title">уровень 1</p>
              <p class="level-xp">250 xp</p>
            </div>
            <div class="level-arrow">></div>
          </button>

          <div class="level-card locked-level">
            <div class="level-text-block">
              <p class="level-title">уровень 2</p>
              <p class="level-xp">250 xp</p>
            </div>
            <div class="level-arrow">></div>
          </div>

          <div class="level-card locked-level">
            <div class="level-text-block">
              <p class="level-title">уровень 3</p>
              <p class="level-xp">250 xp</p>
            </div>
            <div class="level-arrow">></div>
          </div>
        </div>
      </div>

      <div class="footer">
        <button class="back-button" @click="$emit('go-home')">назад</button>
      </div>
    </div>
  </div>
</template>

<script setup>

import oceanImg from '../assets/images/ocean.png'
import dailyImg from '../assets/images/daily.png'
import achievementsImg from '../assets/images/achievements.png'
import profileImg from '../assets/images/profile.png'
import logoImg from '../assets/images/logo.png'
import { gsap } from 'gsap'
import { ref , onMounted } from 'vue';

const props = defineProps ({
    userMode: {
        type: String,
        required: true
    }
})

const emit = defineEmits([
    'open-restricted',
    'open-in-development',
    'open-profile',
    'open-achievements'
])

const handleProfileClick = () => {
    if (props.userMode === 'authorized') {
        emit('open-profile')
    } else {
        emit('open-restricted')
    }
}

const handleAchievementsClick = () => {
    if (props.userMode === 'authorized') {
        emit('open-achievements')
    } else {
        emit('open-restricted')
    }
}
const handleQuestClick = () => {
    emit('open-in-development')
}

const animateOcean = () => {
    const tl = gsap.timeline()

    tl.fromTo(
        '.intro-left',
        {
            opacity: 0,
            y: -40
        },
        {
            opacity: 1,
            y: 1,
            duration: 0.8,
            ease: 'power4.out'

        }
    )
        .fromTo(
            '.intro-right',
            {
                opacity: 0,
                y: -60
            },
            {
                opacity: 1,
                y: 0,
                ease: 'power4.out',
                stagger: 0.12
            },
            '-=0.3'
        )
            .fromTo(
            '.footer',
            {
                opacity: 0,
                y: -30
            },
            {
                opacity: 1,
                y: 0,
                duration: 0.7,
                ease: 'power4.out'
            },
            '-=0.25'
    )
}

onMounted(() => {
    animateOcean()
})

</script>

<style scoped>
.page {
  min-height: 100vh;
  background-image: linear-gradient(to bottom, #0c376a, #042145);
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 30px;
  user-select: none;
}

.logo {
    width: 120px;
    height: auto;
    margin: 0 20px;
}

.header-buttons {
  display: flex;
  gap: 20px;
  align-items: center;
}

.nav-button {
  min-width: 210px;
  height: 60px;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  background-color: #1b334f;
  color: white;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  gap: 12px;
  padding: 0 16px;
  text-align: left;

  transition: background-color 0.25s ease, box-shadow 0.25s ease;
}

.nav-button:hover {
  transform: translateY(-4px) scale(1.03);
  background-color: #042145;
  box-shadow: 0 12px 24px rgba(0,0,0,0.25);
}

.nav-button:active {
  transform: scale(0.97);
  background-color: #1b334f;
}

.nav-icon {
    width: 50px;
    height: auto;
    flex-shrink: 0;
}

.nav-text p {
  margin: 0;
  font-size: 22px;
  font-weight: 900;
  color: white;
  line-height: 1.1;
}

.content {
  padding: 12px 24px 28px;
}

.intro-card {
  max-width: 1360px;
  min-height: 700px;
  margin: 0 auto;
  background-color: #3d73b4;
  border-radius: 42px;
  padding: 40px 36px;
  display: flex;
  gap: 34px;
}

.intro-left {
  width: 520px;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.intro-label {
  margin: 0 0 8px;
  font-size: 24px;
  font-weight: 400;
  color: white;
}

.intro-title {
  margin: 0 0 28px;
  font-size: 52px;
  font-weight: 900;
  color: white;
  line-height: 1.05;
}

.intro-logo {
  width: 220px;
  height: auto;
}

.intro-description {
  margin: 0;
  font-size: 22px;
  font-weight: 400;
  line-height: 1.45;
  color: white;
  text-align: left;
}

.intro-divider {
  width: 4px;
  background-color: #042145;
  border-radius: 10px;
}

.intro-right {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 30px;
}

.level-card {
  width: 100%;
  max-width: 560px;
  min-height: 110px;
  border: none;
  border-radius: 28px;
  background-color: #1b334f;
  padding: 22px 24px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  text-align: left;
  box-shadow: 0 10px 18px rgba(0, 0, 0, 0.18);
}

.current-level {
  cursor: pointer;
  transition: transform 0.25s ease, background-color 0.25s ease, box-shadow 0.25s ease;
}

.current-level:hover {
  transform: translateY(-4px) scale(1.02);
  background-color: #042145;
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.22);
}

.current-level:active {
  transform: scale(0.98);
}

.locked-level {
  filter: blur(2px);
  opacity: 0.85;
  pointer-events: none;
}

.level-text-block {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.level-title {
  margin: 0;
  font-size: 42px;
  font-weight: 900;
  color: white;
  line-height: 1;
}

.level-xp {
  margin: 0;
  font-size: 20px;
  font-weight: 400;
  color: white;
}

.level-arrow {
  width: 54px;
  height: 54px;
  border-radius: 50%;
  background-color: #042145;
  color: white;
  display: flex;
  text-align: center;
  justify-content: center;
  align-items: center;
  font-size: 30px;
  font-weight: 900;
  flex-shrink: 0;
}

.footer {
  display: flex;
  justify-content: center;
  padding-top: 28px;
}

.back-button {
  width: 340px;
  height: 78px;
  border: none;
  border-radius: 20px;
  background-color: #031a36;
  color: white;
  font-size: 44px;
  font-weight: 900;
  cursor: pointer;
  transition: transform 0.25s ease, background-color 0.25s ease, box-shadow 0.25s ease;
}

.back-button:hover {
  background-color: #021429;
  box-shadow: 0 12px 24px rgba(0,0,0,0.25);
}

.back-button:active {
  transform: scale(0.97);
  background-color: #031a36;
}
</style>