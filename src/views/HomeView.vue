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

    <div class="locations">
      <p class="promo-text">выбери, с какой локации начать путешествие!</p>

      <div class="locations-row row-top">
        <button
          v-for="location in topLocations"
          :key="location.id"
          class="location-card"
          :class="{ selected: selectedLocation === location.id }"
          @click="selectLocation(location.id)"
        >
          <div class="selection-indicator" v-if="selectedLocation === location.id">
            <span class="check-icon">✓</span>
          </div>
          <img :src="location.image" :alt="location.title" class="location-icon" />
          <div class="location-text">
            <h3>{{ location.title }}</h3>
            <p>{{ location.description }}</p>
          </div>
          <div class="selected-overlay" v-if="selectedLocation === location.id"></div>
        </button>
      </div>

      <div class="locations-row row-bottom">
        <button 
          v-for="location in bottomLocations"
          :key="location.id" 
          class="location-card"
          :class="{ selected: selectedLocation === location.id }"
          @click="selectLocation(location.id)"
        >
          <div class="selection-indicator" v-if="selectedLocation === location.id">
            <span class="check-icon">✓</span>
          </div>
          <img :src="location.image" :alt="location.title" class="location-icon" />
          <div class="location-text">
            <h3>{{ location.title }}</h3>
            <p>{{ location.description }}</p>
          </div>
          <div class="selected-overlay" v-if="selectedLocation === location.id"></div>
        </button>
      </div>
    </div>

    <div class="footer">
      <button 
        class="start-button"
        :disabled="selectedLocation === null"
        @click="handleStartClick"
      >
        <span class="start-text">начать</span>
        <span class="start-arrow">→</span>
      </button>
    </div>
  </div>
</template>

<style scoped>
/* Анимации для декора */
@keyframes floatLeaf {
  0%, 100% { transform: translateY(0px) rotate(0deg); }
  50% { transform: translateY(-20px) rotate(10deg); }
}

@keyframes floatCircle {
  0%, 100% { transform: translate(0, 0); }
  25% { transform: translate(15px, -15px); }
  50% { transform: translate(-10px, 20px); }
  75% { transform: translate(20px, 10px); }
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

/* Декоративные элементы фона */
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
  opacity: 0.12;
  animation: floatLeaf 10s infinite ease-in-out;
}

.leaf1 { top: 10%; left: 5%; animation-delay: 0s; }
.leaf2 { top: 20%; right: 8%; animation-delay: 1.5s; font-size: 35px; }
.leaf3 { bottom: 15%; left: 10%; animation-delay: 2.5s; font-size: 25px; }
.leaf4 { bottom: 25%; right: 15%; animation-delay: 1s; font-size: 32px; }
.leaf5 { top: 40%; left: 15%; animation-delay: 3s; font-size: 30px; }
.leaf6 { bottom: 40%; right: 20%; animation-delay: 2s; font-size: 28px; }

.circle {
  position: absolute;
  border-radius: 50%;
  background: rgba(50, 180, 144, 0.04);
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

.nav-icon {
  width: 50px;
  height: auto;
  flex-shrink: 0;
}

.nav-button {
  width: 250px;
  height: 60px;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  background: rgba(255, 255, 255, 0.1);
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
  background: rgba(50, 180, 144, 0.2);
  box-shadow: 0 12px 24px rgba(0,0,0,0.25);
  border-color: #32b490;
}

.nav-button:active {
  transform: scale(0.97);
}

.nav-text p {
  margin: 0;
  font-size: 22px;
  line-height: 79%;
  font-weight: 900;
  color: white;
}

.locations {
  margin: 0 50px;
  padding: 0 30px;
  position: relative;
  z-index: 2;
}

.promo-text {
  margin-top: 5px;
  font-size: 40px;
  font-weight: 400;
  color: white;
  margin-bottom: 30px;
  user-select: none;
  text-align: left;
}

.locations-row {
  display: flex;
  justify-content: center;
  gap: 30px;
  position: relative;
  z-index: 1;
  user-select: none;
}

.row-top {
  margin-bottom: 30px;
}

.row-bottom {
  margin-bottom: 40px;
}

.location-card {
  width: 520px;
  height: 245px;
  background: rgba(255, 255, 255, 0.08);
  backdrop-filter: blur(5px);
  color: white;
  border: 2px solid rgba(50, 180, 144, 0.2);
  border-radius: 48px;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-start;
  text-align: left;
  padding: 24px 30px 28px;
  position: relative;
  z-index: 1;
  overflow: hidden;
}

.location-card:hover {
  transform: translateY(-6px) scale(1.02);
  background: rgba(50, 180, 144, 0.15);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
  border-color: #55b49a;
}

.location-card.selected {
  background: linear-gradient(135deg, rgba(50, 180, 144, 0.3), rgba(4, 106, 78, 0.4));
  border: 2px solid #32b490;
  box-shadow: 0 0 0 4px rgba(50, 180, 144, 0.3), 0 20px 40px rgba(0, 0, 0, 0.3);
  transform: translateY(-6px) scale(1.02);
}

.selection-indicator {
  position: absolute;
  top: 20px;
  right: 20px;
  width: 32px;
  height: 32px;
  background: #32b490;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  animation: popIn 0.3s cubic-bezier(0.68, -0.55, 0.265, 1.55);
  z-index: 3;
  box-shadow: 0 0 0 4px rgba(50, 180, 144, 0.3);
}

@keyframes popIn {
  0% { transform: scale(0); opacity: 0; }
  80% { transform: scale(1.2); }
  100% { transform: scale(1); opacity: 1; }
}

.check-icon {
  color: white;
  font-size: 20px;
  font-weight: bold;
}

.selected-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: radial-gradient(circle at 30% 20%, rgba(50, 180, 144, 0.15), transparent);
  pointer-events: none;
  z-index: 2;
  border-radius: 48px;
}

.location-card:active {
  transform: scale(0.98);
}

.location-icon {
  width: 100px;
  height: auto;
  flex-shrink: 0;
  margin-bottom: 20px;
  transition: transform 0.3s ease;
}

.location-card.selected .location-icon {
  transform: scale(1.05);
}

.location-text {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 10px;
  user-select: none;
}

.location-text h3 {
  margin: 0;
  font-size: 28px;
  font-weight: 900;
  line-height: 73%;
  text-transform: uppercase;
  transition: color 0.3s ease;
}

.location-card.selected .location-text h3 {
  color: #55b49a;
}

.location-text p {
  margin: 0;
  font-size: 20px;
  font-weight: 400;
  line-height: 1.3;
}

.footer {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-top: 70px;
  padding-bottom: 40px;
  position: relative;
  z-index: 2;
}

.start-button {
  width: 340px;
  height: 80px;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  font-weight: 900;
  background: linear-gradient(135deg, #32b490 0%, #046a4e 100%);
  color: white;
  transition: all 0.25s ease;
  position: relative;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}

.start-text {
  font-size: 48px;
  transition: transform 0.2s ease;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}

.start-arrow {
  font-size: 40px;
  opacity: 0;
  transform: translateX(-20px);
  transition: all 0.2s ease;
  position: absolute;
  right: 50px;
}

.start-button::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: left 0.4s ease;
}

.start-button:hover::before {
  left: 100%;
}

.start-button:hover:not(:disabled) {
  transform: translateY(-4px) scale(1.02);
  background: linear-gradient(135deg, #55b49a 0%, #32b490 100%);
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.3);
}

.start-button:hover:not(:disabled) .start-text {
  transform: translateX(calc(-50% - 15px));
}

.start-button:hover:not(:disabled) .start-arrow {
  opacity: 1;
  transform: translateX(0);
}

.start-button:active:not(:disabled) {
  transform: scale(0.98);
}

.start-button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.mobile-menu {
  position: fixed;
  top: 0;
  right: -100%;
  width: 280px;
  height: 100vh;
  background: linear-gradient(135deg, #046a4e, #014532);
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
  border: 1px solid rgba(50, 180, 144, 0.3);
}

.mobile-nav-button:hover {
  background: rgba(50, 180, 144, 0.2);
  transform: translateX(5px);
  border-color: #32b490;
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

@media (max-width: 1200px) {
  .location-card { width: 450px; height: auto; min-height: 220px; }
  .location-text h3 { font-size: 24px; }
  .location-text p { font-size: 18px; }
  .promo-text { font-size: 34px; }
}

@media (max-width: 992px) {
  .location-card { width: 400px; }
  .location-icon { width: 80px; }
  .location-text h3 { font-size: 22px; }
  .location-text p { font-size: 16px; }
  .promo-text { font-size: 30px; }
  .nav-button { width: 220px; height: 55px; }
  .nav-text p { font-size: 18px; }
  .nav-icon { width: 40px; }
  .logo { width: 100px; }
}

@media (max-width: 850px) {
  .locations { margin: 0 20px; padding: 0 15px; }
  .location-card { width: calc(50% - 15px); min-width: 280px; }
  .locations-row { flex-wrap: wrap; }
  .leaf, .circle { opacity: 0.06; }
}

@media (max-width: 768px) {
  .locations-row { flex-direction: column; align-items: center; }
  .location-card { width: 100%; max-width: 450px; }
  .row-bottom { margin-bottom: 0; }
  .footer { margin-top: 40px; padding-bottom: 30px; }
  .start-button { width: 280px; height: 70px; }
  .start-text { font-size: 40px; }
  .start-arrow { font-size: 32px; right: 40px; }
  .promo-text { font-size: 28px; }
  .leaf, .circle { opacity: 0.05; }
}

@media (max-width: 576px) {
  .header { padding: 15px; }
  .logo { width: 80px; margin: 0 10px; }
  .burger-button { width: 26px; height: 20px; }
  .locations { margin: 0 15px; padding: 0 10px; }
  .promo-text { font-size: 22px; margin-bottom: 20px; }
  .location-card { padding: 16px; border-radius: 32px; margin-bottom: 15px; }
  .location-icon { width: 60px; margin-bottom: 12px; }
  .location-text h3 { font-size: 18px; }
  .location-text p { font-size: 14px; }
  .footer { margin-top: 30px; padding-bottom: 20px; }
  .start-button { width: 240px; height: 60px; }
  .start-text { font-size: 32px; }
  .start-arrow { font-size: 28px; right: 30px; }
  .mobile-menu { width: 260px; }
  .mobile-nav-button { height: 55px; font-size: 16px; }
  .mobile-nav-icon { width: 30px; }
  .leaf, .circle { display: none; }
}
</style>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { gsap } from 'gsap'
import cityImg from '../assets/images/city.png'
import gardenImg from '../assets/images/garden.png'
import iceImg from '../assets/images/ice.png'
import forestImg from '../assets/images/forest.png'
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
  'open-first-location',
  'open-profile',
  'open-achievements'
])

const selectedLocation = ref(null)
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

const locations = [
  { id: 1, title: 'Друг океанов', description: 'Погрузись в подводный мир и помоги морским обитателям сохранить чистоту океана.', image: oceanImg },
  { id: 2, title: 'Хранитель леса', description: 'Исследуй зелёный лес и узнай, как защищать деревья, животных и лесные тропы.', image: forestImg },
  { id: 3, title: 'Ледяной край', description: 'Отправляйся в мир снега и льда и узнай, почему так важно сохранять холодные экосистемы.', image: iceImg },
  { id: 4, title: 'Сад жизни', description: 'Выращивай растения, наблюдай за природой и учись заботиться о мире вокруг себя.', image: gardenImg },
  { id: 5, title: 'Город будущего', description: 'Узнай, как экология, транспорт и технологии могут сделать город чище и удобнее для всех.', image: cityImg }
]

const topLocations = computed(() => locations.slice(0, 3))
const bottomLocations = computed(() => locations.slice(3))

const selectLocation = (id) => { selectedLocation.value = id }

const handleProfileClick = () => {
  if (props.userMode === 'authorized') emit('open-profile')
  else emit('open-restricted')
}

const handleAchievementsClick = () => {
  if (props.userMode === 'authorized') emit('open-achievements')
  else emit('open-restricted')
}

const handleQuestClick = () => emit('open-in-development')

const handleStartClick = () => {
  if (selectedLocation.value === 1) emit('open-first-location')
  else if (selectedLocation.value !== null) emit('open-in-development')
}

const animateHome = () => {
  const tl = gsap.timeline()
  tl.fromTo('.promo-text', { opacity: 0, y: -40 }, { opacity: 1, y: 1, duration: 0.8, ease: 'power4.out' })
    .fromTo('.location-card', { opacity: 0, y: -60 }, { opacity: 1, y: 0, ease: 'power4.out', stagger: 0.12 }, '-=0.3')
    .fromTo('.start-button', { opacity: 0, y: -30 }, { opacity: 1, y: 0, duration: 0.7, ease: 'power4.out' }, '-=0.25')
}

onMounted(() => { animateHome() })
</script>