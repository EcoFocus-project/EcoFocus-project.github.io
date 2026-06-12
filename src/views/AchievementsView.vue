<template>
  <div class="page">
    <!-- Декоративные элементы фона -->
    <div class="bg-decoration">
      <div class="leaf leaf1">🍃</div>
      <div class="leaf leaf2">🌿</div>
      <div class="leaf leaf3">🍂</div>
      <div class="leaf leaf4">🌱</div>
      <div class="circle circle1"></div>
      <div class="circle circle2"></div>
      <div class="circle circle3"></div>
    </div>

    <div class="header">
      <div class="header-left">
        <img :src="logoImg" alt="логотип" class="logo" />
        <p class="welcome-text">тут хранятся ваши достижения, <span class="nickname">экологист</span>!</p>
      </div>

      <div class="header-buttons">
        <button class="nav-button current-button">
          <img :src="achievementsImg" alt="достижение" class="nav-icon" />
          <div class="nav-text">
            <p>достижения</p>
          </div>
        </button>

        <button class="nav-button" @click="$emit('go-home')">
          <img :src="backImg" alt="назад" class="nav-icon" />
          <div class="nav-text">
            <p>вернуться назад</p>
          </div>
        </button>
      </div>
    </div>

    <div class="achievements-content">
      <div class="achievements-card">
        <div class="top-block">
          <img :src="avatarImg" alt="аватар" class="avatar" />

          <div class="rank-section">
            <h2 class="section-title">ваш ранг</h2>

            <div class="rank-card">
              <div class="rank-top">
                <div class="rank-left">
                  <p class="rank-name">юный эколог</p>
                  <button class="rank-info-button" @click="$emit('open-rank')">→</button>
                </div>

                <img :src="badgeImg" alt="бейдж" class="rank-badge" />
              </div>

              <div class="rank-bottom">
                <div class="xp-bar">
                  <div class="xp-fill" style="width: 0%"></div>
                </div>
                <p class="xp-text">0/1000 xp</p>
              </div>
            </div>
          </div>
        </div>

        <div class="divider"></div>

        <!-- Десктопная версия со стрелками -->
        <div class="slider-section desktop-slider">
          <button
            class="slider-arrow left-arrow"
            :class="{ disabled: !canSlideLeft }"
            @click="slideLeft"
            :disabled="!canSlideLeft"
          >
            ←
          </button>

          <div class="cards-viewport" ref="viewportRef">
            <div 
              class="cards-track"
              :style="{ transform: `translateX(-${currentOffset}px)` }"
            >
              <div
                v-for="achievement in achievements"
                :key="achievement.id"
                class="achievement-card"
                :class="{ locked: achievement.locked }"
              >
                <div class="achievement-icon">{{ achievement.icon }}</div>
                <h3 class="achievement-title">{{ achievement.title }}</h3>
                <p class="achievement-description">{{ achievement.description }}</p>
                <div v-if="achievement.locked" class="lock-overlay">🔒</div>
              </div>
            </div>
          </div>

          <button
            class="slider-arrow right-arrow"
            :class="{ disabled: !canSlideRight }"
            @click="slideRight"
            :disabled="!canSlideRight"
          >
            →
          </button>
        </div>

        <!-- Мобильная версия с горизонтальным скроллом -->
        <div class="slider-section-mobile">
          <div class="cards-viewport-mobile">
            <div class="cards-track-mobile">
              <div
                v-for="achievement in achievements"
                :key="achievement.id"
                class="achievement-card-mobile"
                :class="{ locked: achievement.locked }"
              >
                <div class="achievement-icon">{{ achievement.icon }}</div>
                <h3 class="achievement-title">{{ achievement.title }}</h3>
                <p class="achievement-description">{{ achievement.description }}</p>
                <div v-if="achievement.locked" class="lock-overlay">🔒</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted, nextTick } from 'vue'
import { gsap } from 'gsap'
import avatarImg from '../assets/images/avatar.png'
import badgeImg from '../assets/images/rank_icon.png'
import backImg from '../assets/images/back.png'
import achievementsImg from '../assets/images/achievements.png'
import logoImg from '../assets/images/logo.png'

defineEmits(['go-home', 'open-rank'])

const cardWidth = 300
const gap = 18

const currentIndex = ref(0)
const viewportRef = ref(null)
const cardsPerView = ref(4)
const isMobile = ref(false)

const achievements = [
  {
    id: 1,
    title: 'Друг океанов',
    description: 'Пройдите первую локацию',
    icon: '🌊',
    locked: false
  },
  {
    id: 2,
    title: 'Хранитель леса',
    description: 'Пройдите лесную локацию',
    icon: '🌲',
    locked: true
  },
  {
    id: 3,
    title: 'Ледяной край',
    description: 'Пройдите арктическую локацию',
    icon: '❄️',
    locked: true
  },
  {
    id: 4,
    title: 'Сад жизни',
    description: 'Пройдите экологический сад',
    icon: '🌻',
    locked: true
  },
  {
    id: 5,
    title: 'Город будущего',
    description: 'Пройдите городскую локацию',
    icon: '🏙️',
    locked: true
  },
  {
    id: 6,
    title: 'Защитник природы',
    description: 'Наберите 500 очков опыта',
    icon: '🛡️',
    locked: true
  },
  {
    id: 7,
    title: 'Эко-активист',
    description: 'Пройдите 3 локации',
    icon: '🌍',
    locked: true
  },
  {
    id: 8,
    title: 'Мастер переработки',
    description: 'Правильно рассортируйте 20 предметов',
    icon: '♻️',
    locked: true
  },
  {
    id: 9,
    title: 'Друг планеты',
    description: 'Наберите 1000 очков опыта',
    icon: '🌟',
    locked: true
  }
]

const cardTotalWidth = computed(() => cardWidth + gap)

const maxIndex = computed(() => {
  return Math.max(0, achievements.length - cardsPerView.value)
})

const currentOffset = computed(() => {
  return currentIndex.value * cardTotalWidth.value
})

const canSlideLeft = computed(() => currentIndex.value > 0)
const canSlideRight = computed(() => currentIndex.value < maxIndex.value)

const updateCardsPerView = () => {
  if (viewportRef.value) {
    const containerWidth = viewportRef.value.clientWidth
    let possibleCards = Math.floor(containerWidth / cardTotalWidth.value)
    possibleCards = Math.max(1, Math.min(possibleCards, achievements.length))
    
    if (possibleCards !== cardsPerView.value) {
      cardsPerView.value = possibleCards
      if (currentIndex.value > maxIndex.value) {
        currentIndex.value = maxIndex.value
      }
    }
  }
}

const checkIsMobile = () => {
  isMobile.value = window.innerWidth <= 768
}

const slideLeft = () => {
  if (canSlideLeft.value) {
    currentIndex.value--
  }
}

const slideRight = () => {
  if (canSlideRight.value) {
    currentIndex.value++
  }
}

const animateAchiev = () => {
  const tl = gsap.timeline()
  tl.fromTo('.welcome-text', { opacity: 0, y: -40 }, { opacity: 1, y: 1, duration: 0.8, ease: 'power4.out' })
    .fromTo('.achievements-card', { opacity: 0, y: -60 }, { opacity: 1, y: 0, ease: 'power4.out', stagger: 0.12 }, '-=0.3')
    .fromTo('.slider-section, .slider-section-mobile', { opacity: 0, y: -30 }, { opacity: 1, y: 0, duration: 0.7, ease: 'power4.out' }, '-=0.25')
}

const handleResize = () => {
  checkIsMobile()
  updateCardsPerView()
}

onMounted(() => {
  animateAchiev()
  checkIsMobile()
  nextTick(() => {
    updateCardsPerView()
  })
  window.addEventListener('resize', handleResize)
})

onUnmounted(() => {
  window.removeEventListener('resize', handleResize)
})
</script>

<style scoped>
/* Анимации */
@keyframes floatLeaf {
  0%, 100% { transform: translateY(0px) rotate(0deg); }
  50% { transform: translateY(-15px) rotate(10deg); }
}

@keyframes floatCircle {
  0%, 100% { transform: translate(0, 0); }
  25% { transform: translate(10px, -10px); }
  50% { transform: translate(-5px, 15px); }
  75% { transform: translate(15px, 5px); }
}

@keyframes gentleFloat {
  0%, 100% { transform: translateY(0px); }
  50% { transform: translateY(-6px); }
}

.page {
  min-height: 100vh;
  background: linear-gradient(135deg, #046a4e, #014532);
  position: relative;
  overflow-x: hidden;
}

/* Декоративные элементы */
.bg-decoration {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 0;
  overflow: hidden;
}

.leaf {
  position: absolute;
  font-size: 28px;
  opacity: 0.1;
  animation: floatLeaf 10s infinite ease-in-out;
}

.leaf1 { top: 10%; left: 5%; animation-delay: 0s; }
.leaf2 { top: 20%; right: 8%; animation-delay: 1.5s; font-size: 35px; }
.leaf3 { bottom: 15%; left: 10%; animation-delay: 2.5s; font-size: 25px; }
.leaf4 { bottom: 25%; right: 15%; animation-delay: 1s; font-size: 32px; }

.circle {
  position: absolute;
  border-radius: 50%;
  background: rgba(50, 180, 144, 0.03);
  pointer-events: none;
  animation: floatCircle 15s infinite ease-in-out;
}

.circle1 {
  width: 300px;
  height: 300px;
  top: -100px;
  right: -100px;
}

.circle2 {
  width: 450px;
  height: 450px;
  bottom: -150px;
  left: -150px;
  animation-delay: 2s;
}

.circle3 {
  width: 200px;
  height: 200px;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  animation-delay: 4s;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 30px;
  user-select: none;
  position: relative;
  z-index: 2;
}

.header-left {
  display: flex;
  flex-direction: row;
  gap: 10px;
  align-items: center;
  flex-wrap: wrap;
}

.logo {
  width: 120px;
  height: auto;
  margin: 0 20px;
  animation: gentleFloat 3s ease-in-out infinite;
}

.header-buttons {
  display: flex;
  gap: 20px;
  align-items: center;
}

.nav-button {
  min-width: 230px;
  height: 60px;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  background: rgba(5, 68, 38, 0.4);
  backdrop-filter: blur(5px);
  color: white;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  gap: 12px;
  padding: 0 16px;
  text-align: left;
  transition: all 0.25s ease;
  border: 1px solid rgba(50, 180, 144, 0.3);
}

.nav-button:hover {
  transform: translateY(-4px) scale(1.03);
  background: rgba(1, 59, 43, 0.8);
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.25);
  border-color: #32b490;
}

.nav-button:active {
  transform: scale(0.97);
}

.nav-icon {
  width: 50px;
  height: auto;
  flex-shrink: 0;
}

.nav-text {
  display: flex;
  align-items: center;
}

.nav-text p {
  margin: 0;
  font-size: 24px;
  font-weight: 900;
  color: white;
}

.current-button {
  background: rgba(1, 59, 43, 0.8);
  border-color: #32b490;
}

.achievements-content {
  padding: 10px 60px 40px;
  position: relative;
  z-index: 2;
}

.welcome-text {
  font-size: 36px;
  font-weight: 400;
  color: white;
  user-select: none;
}

.nickname {
  font-weight: 900;
  color: #55b49a;
}

.achievements-card {
  background: rgba(4, 106, 77, 0.8);
  backdrop-filter: blur(5px);
  border-radius: 48px;
  padding: 36px 40px;
  display: flex;
  flex-direction: column;
  gap: 28px;
  margin: 0 auto;
  border: 1px solid rgba(50, 180, 144, 0.3);
}

.top-block {
  display: flex;
  align-items: center;
  gap: 32px;
  flex-wrap: wrap;
}

.avatar {
  width: 120px;
  height: auto;
  animation: gentleFloat 3s ease-in-out infinite;
}

.rank-section {
  flex: 1;
}

.section-title {
  margin: 0 0 16px;
  font-size: 32px;
  font-weight: 900;
  color: #ffffff;
}

.rank-card {
  background: rgba(1, 69, 48, 0.8);
  backdrop-filter: blur(5px);
  border-radius: 28px;
  padding: 18px 22px;
  display: flex;
  flex-direction: column;
  gap: 18px;
  border: 1px solid rgba(50, 180, 144, 0.2);
}

.rank-top {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.rank-left {
  display: flex;
  align-items: center;
  gap: 10px;
}

.rank-name {
  margin: 0;
  font-size: 32px;
  font-weight: 900;
  color: white;
}

.rank-info-button {
  background: transparent;
  border: none;
  padding: 0;
  color: white;
  font-size: 28px;
  font-weight: 900;
  cursor: pointer;
  transition: all 0.2s ease;
}

.rank-info-button:hover {
  transform: translateX(5px);
  color: #32b490;
}

.rank-badge {
  width: 60px;
  height: auto;
  flex-shrink: 0;
}

.rank-bottom {
  display: flex;
  align-items: center;
  gap: 16px;
}

.xp-bar {
  flex: 1;
  height: 14px;
  border-radius: 12px;
  background: rgba(255, 255, 255, 0.2);
  overflow: hidden;
}

.xp-fill {
  height: 100%;
  border-radius: 12px;
  background: linear-gradient(90deg, #32b490, #55b49a);
}

.xp-text {
  margin: 0;
  font-size: 20px;
  color: white;
  font-weight: 400;
}

.divider {
  width: 100%;
  height: 2px;
  background: rgba(50, 180, 144, 0.3);
  border-radius: 10px;
}

/* Десктопная версия со стрелками */
.desktop-slider {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 18px;
}

.slider-arrow {
  width: 48px;
  height: 48px;
  border: none;
  background: rgba(1, 59, 43, 0.8);
  border-radius: 50%;
  color: white;
  font-size: 32px;
  font-weight: 900;
  cursor: pointer;
  flex-shrink: 0;
  transition: all 0.25s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid rgba(50, 180, 144, 0.3);
}

.slider-arrow:hover:not(.disabled) {
  background: #32b490;
  transform: scale(1.05);
  border-color: #55b49a;
}

.slider-arrow.disabled {
  opacity: 0.3;
  cursor: not-allowed;
}

.cards-viewport {
  flex: 1;
  overflow: hidden;
  min-width: 0;
}

.cards-track {
  display: flex;
  gap: 18px;
  transition: transform 0.35s ease;
  will-change: transform;
}

/* Мобильная версия с горизонтальным скроллом */
.slider-section-mobile {
  display: none;
  width: 100%;
  overflow-x: auto;
  overflow-y: hidden;
  -webkit-overflow-scrolling: touch;
  scrollbar-width: thin;
}

.slider-section-mobile::-webkit-scrollbar {
  height: 6px;
}

.slider-section-mobile::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 10px;
}

.slider-section-mobile::-webkit-scrollbar-thumb {
  background: #32b490;
  border-radius: 10px;
}

.cards-viewport-mobile {
  width: 100%;
}

.cards-track-mobile {
  display: flex;
  gap: 18px;
  padding-bottom: 10px;
}

.achievement-card-mobile {
  background: rgba(1, 59, 43, 0.9);
  backdrop-filter: blur(5px);
  border-radius: 28px;
  padding: 24px 20px;
  min-height: 280px;
  width: 280px;
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  border: 1px solid rgba(50, 180, 144, 0.2);
  transition: all 0.25s ease;
  position: relative;
}

.achievement-card-mobile:hover {
  transform: translateY(-5px);
  border-color: #32b490;
}

/* Общие стили для карточек */
.achievement-card,
.achievement-card-mobile {
  background: rgba(1, 59, 43, 0.9);
  backdrop-filter: blur(5px);
  border-radius: 28px;
  padding: 24px 20px;
  min-height: 280px;
  width: 280px;
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  border: 1px solid rgba(50, 180, 144, 0.2);
  transition: all 0.25s ease;
  position: relative;
}

.achievement-card:hover,
.achievement-card-mobile:hover {
  transform: translateY(-5px);
  border-color: #32b490;
}

.achievement-icon {
  font-size: 64px;
  margin-bottom: 20px;
}

.achievement-title {
  margin: 0 0 12px;
  font-size: 24px;
  font-weight: 900;
  color: white;
}

.achievement-description {
  margin: 0;
  font-size: 14px;
  font-weight: 400;
  color: rgba(255, 255, 255, 0.8);
  line-height: 1.35;
}

.locked {
  filter: blur(3px);
  opacity: 0.7;
  position: relative;
}

.lock-overlay {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 48px;
  filter: none;
  background: rgba(0, 0, 0, 0.5);
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 28px;
  backdrop-filter: blur(2px);
}

/* ========== АДАПТИВНОСТЬ ========== */
@media (max-width: 768px) {
  .header {
    padding: 15px;
    flex-direction: column;
    gap: 15px;
  }
  .header-left {
    flex-direction: row;
    align-items: center;
    justify-content: center;
    width: 100%;
  }
  .header-buttons {
    width: 100%;
    justify-content: center;
    flex-wrap: wrap;
  }
  .nav-button {
    min-width: 180px;
    height: 50px;
  }
  .nav-text p {
    font-size: 18px;
  }
  .nav-icon {
    width: 40px;
  }
  .achievements-content {
    padding: 10px 20px 30px;
  }
  .welcome-text {
    font-size: 24px;
  }
  .achievements-card {
    padding: 25px;
    border-radius: 36px;
  }
  .avatar {
    width: 80px;
  }
  .section-title {
    font-size: 28px;
  }
  .rank-name {
    font-size: 28px;
  }
  .rank-badge {
    width: 50px;
  }
  .rank-info-button {
    font-size: 24px;
  }
  .xp-text {
    font-size: 18px;
  }
  
  /* На мобильных показываем скролл, скрываем стрелки */
  .desktop-slider {
    display: none;
  }
  .slider-section-mobile {
    display: block;
  }
  .achievement-card-mobile {
    width: 260px;
    min-height: 260px;
    padding: 20px;
  }
  .achievement-icon {
    font-size: 50px;
  }
  .achievement-title {
    font-size: 22px;
  }
}

@media (max-width: 576px) {
  .header {
    padding: 12px;
  }
  .logo {
    width: 70px;
    margin: 0 10px;
  }
  .welcome-text {
    font-size: 18px;
  }
  .nav-button {
    min-width: 140px;
    height: 45px;
  }
  .nav-text p {
    font-size: 14px;
  }
  .nav-icon {
    width: 30px;
  }
  .achievements-content {
    padding: 10px 15px 20px;
  }
  .achievements-card {
    padding: 20px;
    border-radius: 28px;
    gap: 20px;
  }
  .avatar {
    width: 70px;
  }
  .section-title {
    font-size: 24px;
    margin-bottom: 12px;
  }
  .rank-name {
    font-size: 22px;
  }
  .rank-badge {
    width: 40px;
  }
  .rank-info-button {
    font-size: 22px;
  }
  .xp-text {
    font-size: 14px;
  }
  .achievement-card-mobile {
    width: 240px;
    min-height: 240px;
    padding: 16px;
  }
  .achievement-icon {
    font-size: 40px;
    margin-bottom: 12px;
  }
  .achievement-title {
    font-size: 20px;
  }
  .achievement-description {
    font-size: 13px;
  }
  .leaf, .circle {
    opacity: 0.05;
  }
}

@media (max-width: 380px) {
  .achievement-card-mobile {
    width: 220px;
    min-height: 220px;
  }
  .achievement-icon {
    font-size: 36px;
  }
  .achievement-title {
    font-size: 18px;
  }
}
</style>