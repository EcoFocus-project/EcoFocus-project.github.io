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
        <p class="welcome-text">с возвращением, <span class="nickname-placeholder">экологист</span>!</p>
      </div>

      <div class="header-buttons">
        <button class="nav-button current-button">
          <div class="nav-text">
            <img :src="profileImg" alt="профиль" class="nav-icon" />
            <p>мой профиль</p>
          </div>
        </button>

        <button class="nav-button" @click="$emit('go-home')">
          <div class="nav-text">
            <img :src="backImg" alt="назад" class="nav-icon" />
            <p>вернуться назад</p>
          </div>
        </button>
      </div>
    </div>

    <div class="profile-content">
      <div class="profile-card">
        <div class="profile-left">
          <div class="avatar-block">
            <img :src="avatarImg" alt="аватар" class="avatar" />
          </div>

          <div class="profile-actions">
            <button class="action-button" @click="$emit('open-edit-profile')">редактировать профиль</button>
            <button class="action-button" @click="$emit('open-account-settings')">настройки аккаунта</button>
            <button class="action-button" @click="$emit('open-logout')">выйти из аккаунта</button>
          </div>
        </div>

        <div class="profile-divider"></div>

        <div class="profile-right">
          <div class="rank-section">
            <h2 class="section-title">ваш ранг</h2>

            <div class="rank-card">
              <div class="rank-top">
                <div class="rank-left">
                  <p class="rank-name">юный эколог</p>
                  <button class="rank-info-button" @click="$emit('open-rank')">→</button>
                </div>
                
                <img :src="badgeImg" alt="бейдж" class="rank-badge">
              </div>

              <div class="rank-bottom">
                <div class="xp-bar">
                  <div class="xp-fill" style="width: 0%;"></div>
                </div>
                <p class="xp-text">0/1000 xp</p>
              </div>
            </div>
          </div>

          <div class="stats-section">
            <h2 class="section-title">статистика</h2>

            <div class="stats-grid">
              <div class="stat-card">
                <img :src="statDayImg" alt="1" class="stat-icon" />
                <h3 class="stat-title">1 день</h3>
                <p class="stat-subtitle">вы заходите подряд.</p>
                <p class="stat-link-text">ваша серия за все время -<br> 1 день</p>
              </div>

              <div class="stat-card">
                <img :src="statLocImg" alt="1" class="stat-icon" />
                <h3 class="stat-title">0/5</h3>
                <p class="stat-subtitle">пройденных локаций!</p>
                <p class="stat-link-text">
                  продолжим путешествие 
                  <button class="inline-link" @click="$emit('go-home')">вместе?</button>
                </p>
              </div>

              <div class="stat-card">
                <img :src="statAchImg" alt="1" class="stat-icon" />
                <h3 class="stat-title">друг океанов</h3>
                <p class="stat-subtitle">недавняя награда</p>
                <p class="stat-link-text">
                  вы можете изучить другие
                  <button class="inline-link" @click="$emit('open-achievements')">достижения</button>
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

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

.welcome-text {
  font-size: 36px;
  font-weight: 400;
  color: white;
  user-select: none;
}

.nickname-placeholder {
  font-weight: 900;
  color: #55b49a;
}

.header-buttons {
  display: flex;
  gap: 20px;
  align-items: center;
}

.nav-button {
  min-width: 260px;
  height: 60px;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  background: rgba(5, 68, 38, 0.4);
  backdrop-filter: blur(5px);
  color: white;
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
  padding-right: 10px;
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

.profile-content {
  padding: 10px 70px 40px;
  position: relative;
  z-index: 2;
}

.profile-card {
  background: rgba(4, 106, 77, 0.8);
  backdrop-filter: blur(5px);
  border-radius: 48px;
  padding: 36px 40px;
  display: flex;
  gap: 28px;
  margin: 0 auto;
  border: 1px solid rgba(50, 180, 144, 0.3);
}

.profile-left {
  width: 400px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.avatar-block {
  position: relative;
  margin-bottom: 40px;
}

.avatar {
  width: 300px;
  height: auto;
  animation: gentleFloat 3s ease-in-out infinite;
}

.profile-actions {
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 20px;
  align-items: center;
}

.action-button {
  width: 270px;
  min-height: 60px;
  border: none;
  border-radius: 20px;
  background: rgba(1, 59, 43, 0.9);
  color: white;
  font-size: 20px;
  font-weight: 900;
  cursor: pointer;
  padding: 12px 16px;
  line-height: 1.2;
  transition: all 0.25s ease;
  backdrop-filter: blur(5px);
  border: 1px solid rgba(50, 180, 144, 0.2);
}

.action-button:hover {
  background: rgba(1, 45, 33, 0.95);
  transform: translateY(-3px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
  border-color: #32b490;
}

.action-button:active {
  transform: scale(0.97);
}

.profile-divider {
  width: 4px;
  background: rgba(50, 180, 144, 0.3);
  border-radius: 10px;
}

.profile-right {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 32px;
}

.section-title {
  margin: 0 0 16px;
  font-size: 40px;
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
  font-size: 40px;
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
  width: 70px;
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
  height: 16px;
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

.stats-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 18px;
}

.stat-card {
  background: rgba(1, 69, 48, 0.8);
  backdrop-filter: blur(5px);
  border-radius: 28px;
  padding: 20px 18px 24px;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  border: 1px solid rgba(50, 180, 144, 0.2);
  transition: all 0.25s ease;
}

.stat-card:hover {
  transform: translateY(-5px);
  border-color: #32b490;
}

.stat-title {
  margin: 0 0 10px;
  font-size: 36px;
  font-weight: 900;
  color: white;
}

.stat-subtitle {
  margin: 0 0 18px;
  font-size: 22px;
  font-weight: 400;
  color: white;
}

.stat-icon {
  width: 120px;
  height: auto;
  margin-bottom: 18px;
}

.stat-link-text {
  margin: 0;
  font-size: 16px;
  font-weight: 300;
  color: #fff;
}

.inline-link {
  background: transparent;
  border: none;
  padding: 0;
  margin: 0 2px;
  color: #32b490;
  font-size: inherit;
  font-weight: 500;
  cursor: pointer;
  text-decoration: underline;
  transition: all 0.2s ease;
}

.inline-link:hover {
  color: #55b49a;
}

/* ========== АДАПТИВНОСТЬ ========== */
@media (max-width: 1200px) {
  .profile-content {
    padding: 10px 40px 40px;
  }
  .profile-left {
    width: 320px;
  }
  .avatar {
    width: 240px;
  }
  .action-button {
    width: 240px;
    font-size: 18px;
  }
  .section-title {
    font-size: 34px;
  }
  .rank-name {
    font-size: 34px;
  }
  .stat-title {
    font-size: 30px;
  }
  .stat-subtitle {
    font-size: 18px;
  }
  .stat-icon {
    width: 100px;
  }
}

@media (max-width: 992px) {
  .profile-content {
    padding: 10px 30px 30px;
  }
  .profile-card {
    flex-direction: column;
    align-items: center;
    padding: 30px;
  }
  .profile-left {
    width: 100%;
    max-width: 400px;
  }
  .profile-divider {
    width: 80%;
    height: 4px;
    margin: 20px 0;
  }
  .profile-right {
    width: 100%;
  }
  .stats-grid {
    grid-template-columns: repeat(2, 1fr);
  }
  .header-left {
    flex-direction: column;
    align-items: flex-start;
  }
  .welcome-text {
    font-size: 28px;
    margin-left: 20px;
  }
  .logo {
    width: 100px;
    margin: 0 20px;
  }
  .nav-button {
    min-width: 200px;
    height: 55px;
  }
  .nav-text p {
    font-size: 20px;
  }
  .nav-icon {
    width: 40px;
  }
}

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
  }
  .profile-content {
    padding: 10px 20px 30px;
  }
  .profile-card {
    padding: 25px 20px;
    border-radius: 36px;
  }
  .avatar {
    width: 200px;
  }
  .action-button {
    width: 220px;
    font-size: 16px;
    min-height: 50px;
  }
  .profile-actions {
    gap: 15px;
  }
  .section-title {
    font-size: 30px;
  }
  .rank-name {
    font-size: 28px;
  }
  .rank-badge {
    width: 55px;
  }
  .rank-info-button {
    font-size: 24px;
  }
  .xp-text {
    font-size: 18px;
  }
  .stats-grid {
    grid-template-columns: 1fr;
    gap: 15px;
  }
  .stat-card {
    max-width: 100%;
  }
  .stat-title {
    font-size: 28px;
  }
  .stat-subtitle {
    font-size: 18px;
  }
  .stat-icon {
    width: 90px;
  }
  .welcome-text {
    font-size: 24px;
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
  .profile-content {
    padding: 10px 15px 20px;
  }
  .profile-card {
    padding: 20px 16px;
    border-radius: 28px;
  }
  .avatar {
    width: 160px;
    margin-bottom: 20px;
  }
  .avatar-block {
    margin-bottom: 25px;
  }
  .action-button {
    width: 200px;
    font-size: 14px;
    min-height: 45px;
    padding: 10px 12px;
  }
  .profile-actions {
    gap: 12px;
  }
  .section-title {
    font-size: 24px;
    margin-bottom: 10px;
  }
  .rank-card {
    padding: 14px 18px;
  }
  .rank-name {
    font-size: 22px;
  }
  .rank-badge {
    width: 45px;
  }
  .rank-info-button {
    font-size: 20px;
  }
  .xp-bar {
    height: 12px;
  }
  .xp-text {
    font-size: 14px;
  }
  .stat-title {
    font-size: 24px;
  }
  .stat-subtitle {
    font-size: 16px;
  }
  .stat-icon {
    width: 80px;
  }
  .stat-link-text {
    font-size: 13px;
  }
  .welcome-text {
    font-size: 18px;
  }
  .nav-button {
    min-width: 170px;
    height: 48px;
  }
  .nav-text p {
    font-size: 16px;
  }
  .nav-icon {
    width: 35px;
  }
  .leaf, .circle {
    opacity: 0.05;
  }
}

@media (max-width: 380px) {
  .logo {
    width: 60px;
  }
  .welcome-text {
    font-size: 16px;
  }
  .avatar {
    width: 130px;
  }
  .action-button {
    width: 180px;
    font-size: 13px;
  }
  .rank-name {
    font-size: 18px;
  }
  .rank-badge {
    width: 38px;
  }
  .stat-title {
    font-size: 22px;
  }
  .stat-subtitle {
    font-size: 14px;
  }
  .stat-icon {
    width: 70px;
  }
  .nav-button {
    min-width: 150px;
    height: 44px;
  }
  .nav-text p {
    font-size: 14px;
  }
  .nav-icon {
    width: 30px;
  }
}
</style>

<script setup>
import { ref, onMounted } from 'vue'
import { gsap } from 'gsap'
import badgeImg from '../assets/images/rank_icon.png'
import statAchImg from '../assets/images/stat_3.png'
import statLocImg from '../assets/images/stat_2.png'
import statDayImg from '../assets/images/stat_1.png'
import avatarImg from '../assets/images/avatar.png'
import backImg from '../assets/images/back.png'
import profileImg from '../assets/images/profile.png'
import logoImg from '../assets/images/logo.png'

const emit = defineEmits([
  'go-home', 
  'open-achievements', 
  'open-logout', 
  'open-rank', 
  'open-account-settings', 
  'open-edit-profile'
])

const animateProfile = () => {
  const tl = gsap.timeline()
  tl.fromTo('.welcome-text', { opacity: 0, y: -40 }, { opacity: 1, y: 1, duration: 0.8, ease: 'power4.out' })
    .fromTo('.rank-card', { opacity: 0, y: -60 }, { opacity: 1, y: 0, ease: 'power4.out', stagger: 0.12 }, '-=0.3')
    .fromTo('.stats-section', { opacity: 0, y: -30 }, { opacity: 1, y: 0, duration: 0.7, ease: 'power4.out' }, '-=0.25')
}

onMounted(() => {
  animateProfile()
})
</script>