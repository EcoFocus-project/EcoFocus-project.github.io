<template>
  <div class="page">
    <!-- Декоративные элементы фона - морская тематика -->
    <div class="bg-decoration">
      <div class="wave wave1"></div>
      <div class="wave wave2"></div>
      <div class="wave wave3"></div>
      <div class="bubble bubble1">💧</div>
      <div class="bubble bubble2">💧</div>
      <div class="bubble bubble3">💧</div>
      <div class="bubble bubble4">💧</div>
      <div class="sea-creature fish1">🐟</div>
      <div class="sea-creature fish2">🐠</div>
      <div class="sea-creature fish3">🐡</div>
      <div class="sea-creature jelly1">🪼</div>
      <div class="sea-creature jelly2">🪼</div>
      <div class="sea-creature star">⭐</div>
      <div class="circle circle1"></div>
      <div class="circle circle2"></div>
      <div class="circle circle3"></div>
    </div>

    <div class="header">
      <img :src="logoImg" alt="логотип" class="logo" />
      
      <button class="burger-button" @click="toggleMenu" :class="{ active: isMenuOpen }">
        <span></span>
        <span></span>
        <span></span>
      </button>

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

    <div class="mobile-menu" :class="{ open: isMenuOpen }">
      <button class="mobile-nav-button" @click="handleProfileClickMobile">
        <img :src="profileImg" alt="профиль" class="mobile-nav-icon" />
        <span>мой профиль</span>
      </button>
      <button class="mobile-nav-button" @click="handleAchievementsClickMobile">
        <img :src="achievementsImg" alt="достижение" class="mobile-nav-icon" />
        <span>достижения</span>
      </button>
      <button class="mobile-nav-button" @click="handleQuestClickMobile">
        <img :src="dailyImg" alt="ежедневное задание" class="mobile-nav-icon" />
        <span>ежедневное задание</span>
      </button>
    </div>

    <div class="overlay" :class="{ visible: isMenuOpen }" @click="closeMenu"></div>

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
            <div class="level-arrow">→</div>
          </button>

          <div class="level-card locked-level">
            <div class="level-text-block">
              <p class="level-title">уровень 2</p>
              <p class="level-xp">250 xp</p>
            </div>
            <div class="level-arrow">→</div>
          </div>

          <div class="level-card locked-level">
            <div class="level-text-block">
              <p class="level-title">уровень 3</p>
              <p class="level-xp">250 xp</p>
            </div>
            <div class="level-arrow">→</div>
          </div>
        </div>
      </div>

      <div class="footer">
        <button class="back-button" @click="$emit('go-home')">
          <span class="back-text">назад</span>
          <span class="back-arrow">←</span>
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { gsap } from 'gsap'
import oceanImg from '../assets/images/ocean.png'
import dailyImg from '../assets/images/daily.png'
import achievementsImg from '../assets/images/achievements.png'
import profileImg from '../assets/images/profile.png'
import logoImg from '../assets/images/logo.png'

const props = defineProps({
  userMode: {
    type: String,
    required: true
  }
})

const emit = defineEmits([
  'open-restricted',
  'open-in-development',
  'open-profile',
  'open-achievements',
  'open-level-one',
  'go-home'
])

const isMenuOpen = ref(false)

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value
  if (isMenuOpen.value) {
    document.body.style.overflow = 'hidden'
  } else {
    document.body.style.overflow = ''
  }
}

const closeMenu = () => {
  isMenuOpen.value = false
  document.body.style.overflow = ''
}

const handleProfileClickMobile = () => {
  closeMenu()
  handleProfileClick()
}

const handleAchievementsClickMobile = () => {
  closeMenu()
  handleAchievementsClick()
}

const handleQuestClickMobile = () => {
  closeMenu()
  handleQuestClick()
}

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
  tl.fromTo('.intro-left', { opacity: 0, y: -40 }, { opacity: 1, y: 1, duration: 0.8, ease: 'power4.out' })
    .fromTo('.intro-right', { opacity: 0, y: -60 }, { opacity: 1, y: 0, ease: 'power4.out', stagger: 0.12 }, '-=0.3')
    .fromTo('.footer', { opacity: 0, y: -30 }, { opacity: 1, y: 0, duration: 0.7, ease: 'power4.out' }, '-=0.25')
}

onMounted(() => {
  animateOcean()
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

@keyframes swim {
  0% { transform: translateX(-30px) translateY(0px); }
  50% { transform: translateX(30px) translateY(-20px); }
  100% { transform: translateX(-30px) translateY(0px); }
}

@keyframes swimReverse {
  0% { transform: translateX(30px) translateY(0px); }
  50% { transform: translateX(-30px) translateY(-15px); }
  100% { transform: translateX(30px) translateY(0px); }
}

@keyframes floatJelly {
  0%, 100% { transform: translateY(0px) scale(1); }
  50% { transform: translateY(-20px) scale(1.05); }
}

@keyframes bubbleFloat {
  0% { transform: translateY(0) scale(0.8); opacity: 0.6; }
  100% { transform: translateY(-100vh) scale(1.2); opacity: 0; }
}

@keyframes waveMove {
  0% { background-position-x: 0; }
  100% { background-position-x: 1000px; }
}

.page {
  min-height: 100vh;
  background: linear-gradient(135deg, #0c376a, #042145);
  position: relative;
  overflow-x: hidden;
}

/* Декоративные элементы - морская тематика */
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

/* Волны */
.wave {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 100px;
  background-repeat: repeat-x;
  opacity: 0.15;
}

.wave1 {
  background: linear-gradient(transparent 70%, rgba(64, 224, 208, 0.3) 100%);
  animation: waveMove 8s linear infinite;
  bottom: 0;
}

.wave2 {
  background: linear-gradient(transparent 80%, rgba(72, 209, 204, 0.2) 100%);
  animation: waveMove 12s linear infinite reverse;
  bottom: 20px;
}

.wave3 {
  background: linear-gradient(transparent 85%, rgba(32, 178, 170, 0.15) 100%);
  animation: waveMove 16s linear infinite;
  bottom: 40px;
}

/* Пузырьки */
.bubble {
  position: absolute;
  font-size: 20px;
  animation: bubbleFloat 6s infinite ease-in;
}

.bubble1 { left: 10%; animation-delay: 0s; }
.bubble2 { left: 30%; animation-delay: 2s; font-size: 16px; }
.bubble3 { left: 60%; animation-delay: 1s; font-size: 24px; }
.bubble4 { left: 85%; animation-delay: 3s; font-size: 18px; }

/* Морские обитатели */
.sea-creature {
  position: absolute;
  font-size: 30px;
  opacity: 0.15;
}

.fish1 { top: 15%; left: -20px; animation: swim 12s infinite ease-in-out; }
.fish2 { top: 50%; right: -20px; animation: swimReverse 14s infinite ease-in-out; font-size: 35px; }
.fish3 { top: 70%; left: -15px; animation: swim 16s infinite ease-in-out; font-size: 25px; }
.jelly1 { top: 30%; right: 10%; animation: floatJelly 8s infinite ease-in-out; font-size: 28px; }
.jelly2 { bottom: 20%; left: 15%; animation: floatJelly 10s infinite ease-in-out reverse; font-size: 32px; }
.star { bottom: 10%; right: 20%; animation: gentleFloat 6s infinite; font-size: 24px; }

.circle {
  position: absolute;
  border-radius: 50%;
  background: rgba(64, 224, 208, 0.03);
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
  padding: 10px 30px;
  user-select: none;
  position: relative;
  z-index: 2;
}

.logo {
  width: 120px;
  height: auto;
  margin: 0 20px;
  animation: gentleFloat 3s ease-in-out infinite;
}

.burger-button {
  display: none;
  flex-direction: column;
  justify-content: space-between;
  width: 30px;
  height: 22px;
  background: transparent;
  border: none;
  cursor: pointer;
  padding: 0;
  z-index: 102;
}

.burger-button span {
  width: 100%;
  height: 3px;
  background: white;
  border-radius: 3px;
  transition: all 0.3s ease;
}

.burger-button.active span:nth-child(1) {
  transform: translateY(9px) rotate(45deg);
}

.burger-button.active span:nth-child(2) {
  opacity: 0;
}

.burger-button.active span:nth-child(3) {
  transform: translateY(-9px) rotate(-45deg);
}

@media (max-width: 992px) {
  .burger-button {
    display: flex !important;
  }
}

.header-buttons {
  display: flex;
  gap: 20px;
  align-items: center;
}

@media (max-width: 992px) {
  .header-buttons {
    display: none;
  }
}

.nav-button {
  min-width: 210px;
  height: 60px;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  background: rgba(27, 51, 79, 0.9);
  backdrop-filter: blur(5px);
  color: white;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  gap: 12px;
  padding: 0 16px;
  text-align: left;
  transition: all 0.25s ease;
  border: 1px solid rgba(64, 224, 208, 0.2);
}

.nav-button:hover {
  transform: translateY(-4px) scale(1.03);
  background-color: #042145;
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.25);
  border-color: rgba(64, 224, 208, 0.5);
}

.nav-button:active {
  transform: scale(0.97);
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

.mobile-menu {
  position: fixed;
  top: 0;
  right: -100%;
  width: 280px;
  height: 100vh;
  background: linear-gradient(135deg, #0c376a, #042145);
  padding: 80px 20px 30px;
  display: flex;
  flex-direction: column;
  gap: 15px;
  z-index: 1000;
  transition: right 0.3s ease;
  box-shadow: -5px 0 25px rgba(0, 0, 0, 0.3);
}

.mobile-menu.open {
  right: 0;
}

.mobile-nav-button {
  width: 100%;
  height: 60px;
  border: none;
  border-radius: 15px;
  cursor: pointer;
  background: rgba(255, 255, 255, 0.1);
  color: white;
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 0 20px;
  text-align: left;
  transition: all 0.25s ease;
  font-size: 18px;
  font-weight: 500;
  border: 1px solid rgba(64, 224, 208, 0.2);
}

.mobile-nav-button:hover {
  background: rgba(64, 224, 208, 0.1);
  transform: translateX(5px);
  border-color: rgba(64, 224, 208, 0.5);
}

.mobile-nav-icon {
  width: 35px;
  height: auto;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(8px);
  z-index: 999;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.overlay.visible {
  opacity: 1;
  visibility: visible;
}

.content {
  padding: 12px 24px 28px;
  position: relative;
  z-index: 2;
}

.intro-card {
  max-width: 1360px;
  margin: 0 auto;
  background: rgba(61, 115, 180, 0.8);
  backdrop-filter: blur(5px);
  border-radius: 42px;
  padding: 40px 36px;
  display: flex;
  gap: 34px;
  border: 1px solid rgba(64, 224, 208, 0.2);
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
  animation: gentleFloat 3s ease-in-out infinite;
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
  background: rgba(64, 224, 208, 0.3);
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
  background: rgba(27, 51, 79, 0.8);
  backdrop-filter: blur(5px);
  padding: 22px 24px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  text-align: left;
  box-shadow: 0 10px 18px rgba(0, 0, 0, 0.18);
  border: 1px solid rgba(64, 224, 208, 0.2);
  transition: all 0.25s ease;
}

.current-level {
  cursor: pointer;
}

.current-level:hover {
  transform: translateY(-4px) scale(1.02);
  background: #042145;
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.22);
  border-color: rgba(64, 224, 208, 0.5);
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
  background: #042145;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 30px;
  font-weight: 900;
  flex-shrink: 0;
  transition: all 0.25s ease;
}

.current-level:hover .level-arrow {
  transform: translateX(5px);
  background: #0c376a;
}

.footer {
  display: flex;
  justify-content: center;
  margin-top: 40px;
  padding-bottom: 20px;
}

.back-button {
  width: 340px;
  height: 78px;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  font-weight: 900;
  background: linear-gradient(135deg, #1b5e8c, #0c376a);
  color: white;
  transition: all 0.25s ease;
  position: relative;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}

.back-text {
  font-size: 44px;
  transition: transform 0.2s ease;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}

.back-arrow {
  font-size: 36px;
  opacity: 0;
  transform: translateX(15px);
  transition: all 0.2s ease;
  position: absolute;
  left: 60px;
}

.back-button::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(64, 224, 208, 0.2), transparent);
  transition: left 0.4s ease;
}

.back-button:hover::before {
  left: 100%;
}

.back-button:hover {
  transform: translateY(-4px) scale(1.02);
  background: linear-gradient(135deg, #0c376a, #1b5e8c);
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.3);
}

.back-button:hover .back-text {
  transform: translateX(calc(-50% + 15px));
}

.back-button:hover .back-arrow {
  opacity: 1;
  transform: translateX(0);
}

.back-button:active {
  transform: scale(0.98);
}

@media (max-width: 1200px) {
  .intro-card {
    padding: 30px;
    gap: 25px;
  }
  .intro-left {
    width: 450px;
  }
  .intro-title {
    font-size: 44px;
  }
  .intro-description {
    font-size: 20px;
  }
  .level-title {
    font-size: 36px;
  }
}

@media (max-width: 992px) {
  .intro-card {
    flex-direction: column;
    align-items: center;
  }
  .intro-left {
    width: 100%;
    max-width: 500px;
  }
  .intro-divider {
    width: 80%;
    height: 4px;
    margin: 20px auto;
  }
  .intro-right {
    width: 100%;
    align-items: center;
  }
  .level-card {
    max-width: 100%;
  }
  .nav-button {
    min-width: 180px;
    height: 55px;
  }
  .nav-text p {
    font-size: 18px;
  }
  .nav-icon {
    width: 40px;
  }
  .logo {
    width: 100px;
  }
  .footer {
    margin-top: 35px;
  }
}

@media (max-width: 768px) {
  .header {
    padding: 15px;
  }
  .content {
    padding: 12px 16px 28px;
  }
  .intro-card {
    padding: 25px 20px;
  }
  .intro-title {
    font-size: 36px;
    margin-bottom: 20px;
  }
  .intro-logo {
    width: 160px;
  }
  .intro-description {
    font-size: 18px;
  }
  .level-title {
    font-size: 32px;
  }
  .level-xp {
    font-size: 18px;
  }
  .level-arrow {
    width: 48px;
    height: 48px;
    font-size: 26px;
  }
  .back-button {
    width: 280px;
    height: 70px;
  }
  .back-text {
    font-size: 36px;
  }
  .back-arrow {
    font-size: 30px;
    left: 50px;
  }
  .footer {
    margin-top: 30px;
  }
  .fish1, .fish2, .fish3, .jelly1, .jelly2, .star {
    opacity: 0.08;
  }
}

@media (max-width: 576px) {
  .header {
    padding: 12px;
  }
  .logo {
    width: 80px;
    margin: 0 10px;
  }
  .burger-button {
    width: 26px;
    height: 20px;
  }
  .content {
    padding: 12px 12px 20px;
  }
  .intro-card {
    padding: 20px 16px;
    border-radius: 32px;
  }
  .intro-label {
    font-size: 18px;
  }
  .intro-title {
    font-size: 28px;
    margin-bottom: 15px;
  }
  .intro-logo {
    width: 120px;
  }
  .intro-description {
    font-size: 16px;
    line-height: 1.35;
  }
  .level-card {
    padding: 16px 20px;
    min-height: 90px;
  }
  .level-title {
    font-size: 28px;
  }
  .level-xp {
    font-size: 16px;
  }
  .level-arrow {
    width: 42px;
    height: 42px;
    font-size: 24px;
  }
  .footer {
    margin-top: 25px;
    padding-bottom: 15px;
  }
  .back-button {
    width: 240px;
    height: 60px;
  }
  .back-text {
    font-size: 30px;
  }
  .back-arrow {
    font-size: 24px;
    left: 40px;
  }
  .mobile-menu {
    width: 260px;
  }
  .mobile-nav-button {
    height: 55px;
    font-size: 16px;
  }
  .mobile-nav-icon {
    width: 30px;
  }
  .sea-creature, .bubble {
    display: none;
  }
  .circle {
    opacity: 0.03;
  }
}

@media (max-width: 380px) {
  .intro-title {
    font-size: 24px;
  }
  .intro-logo {
    width: 100px;
  }
  .intro-description {
    font-size: 14px;
  }
  .level-title {
    font-size: 24px;
  }
  .level-xp {
    font-size: 14px;
  }
  .level-arrow {
    width: 36px;
    height: 36px;
    font-size: 20px;
  }
  .back-button {
    width: 220px;
    height: 55px;
  }
  .back-text {
    font-size: 28px;
  }
  .back-arrow {
    font-size: 22px;
    left: 35px;
  }
  .footer {
    margin-top: 20px;
  }
}
</style>