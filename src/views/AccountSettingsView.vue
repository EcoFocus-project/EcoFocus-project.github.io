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
        <p class="welcome-text">настраиваем ваш аккаунт..</p>
      </div>

      <div class="header-buttons">
        <button class="nav-button">
          <img :src="profileImg" alt="профиль" class="nav-icon" />
          <div class="nav-text">
            <p>мой профиль</p>
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

    <div class="settings-content">
      <div class="settings-card">
        <div class="settings-left">
          <div class="avatar-block">
            <img :src="avatarImg" alt="аватар" class="avatar" />
          </div>

          <div class="profile-actions">
            <button class="action-button" @click="$emit('open-edit-profile')">
              редактировать профиль
            </button>

            <button class="action-button current-button">
              настройки аккаунта
            </button>

            <button class="action-button" @click="$emit('open-logout')">
              выйти из аккаунта
            </button>
          </div>
        </div>

        <div class="settings-divider"></div>

        <div class="settings-right">
          <div class="settings-section">
            <h2 class="section-title">настройки приватности</h2>

            <label class="private-row">
              <input type="checkbox" v-model="isPrivate" class="private-checkbox" />
              <span class="private-label">приватный аккаунт</span>
            </label>
          </div>

          <div class="settings-section">
            <h2 class="section-title">настройки конфиденциальности</h2>

            <div class="password-group">
              <label class="field-label">смена пароля</label>
              <input
                v-model="oldPassword"
                type="password"
                class="password-input"
                placeholder="старый пароль"
              />
              <input
                v-model="newPassword"
                type="password"
                class="password-input"
                placeholder="новый пароль"
              />
            </div>
          </div>

          <div class="settings-section account-section">
            <h2 class="section-title">аккаунт</h2>

            <button class="deactivate-button" @click="$emit('open-deactivate')">
              деактивировать аккаунт
            </button>
          </div>

          <div class="buttons-row">
            <button class="cancel-button" @click="$emit('open-profile')">
              отменить
            </button>
            <button class="save-button" @click="$emit('open-profile')">
              сохранить
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import avatarImg from '../assets/images/avatar.png'
import backImg from '../assets/images/back.png'
import profileImg from '../assets/images/profile.png'
import logoImg from '../assets/images/logo.png'

defineEmits([
  'open-profile',
  'open-edit-profile',
  'open-logout',
  'open-deactivate',
  'go-home'
])

const isPrivate = ref(false)
const oldPassword = ref('')
const newPassword = ref('')
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
  padding-right: 10px;
}

.nav-text p {
  margin: 0;
  font-size: 24px;
  font-weight: 900;
  color: white;
}

.settings-content {
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

.settings-card {
  background: rgba(4, 106, 77, 0.8);
  backdrop-filter: blur(5px);
  border-radius: 48px;
  padding: 36px 40px;
  display: flex;
  gap: 28px;
  margin: 0 auto;
  border: 1px solid rgba(50, 180, 144, 0.3);
}

.settings-left {
  width: 400px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.avatar-block {
  position: relative;
  margin-bottom: 56px;
}

.avatar {
  width: 280px;
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

/* Единый стиль для всех кнопок в левой панели */
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

.current-button {
  background: rgba(1, 45, 33, 0.95);
  border-color: #32b490;
}

.settings-divider {
  width: 4px;
  background: rgba(50, 180, 144, 0.3);
  border-radius: 10px;
}

.settings-right {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.settings-section {
  display: flex;
  flex-direction: column;
  gap: 14px;
}

.section-title {
  margin: 0;
  font-size: 28px;
  font-weight: 900;
  color: #ffffff;
}

.private-row {
  display: flex;
  align-items: center;
  gap: 14px;
  cursor: pointer;
  user-select: none;
}

.private-checkbox {
  width: 24px;
  height: 24px;
  accent-color: #32b490;
  cursor: pointer;
}

.private-label {
  font-size: 20px;
  font-weight: 400;
  color: #ffffff;
}

.password-group {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.field-label {
  margin: 0;
  font-size: 20px;
  font-weight: 500;
  color: #ffffff;
  letter-spacing: 0.5px;
}

.password-input {
  width: 100%;
  max-width: 460px;
  height: 52px;
  border: 1px solid rgba(50, 180, 144, 0.3);
  border-radius: 14px;
  background: rgba(255, 255, 255, 0.1);
  color: white;
  font-size: 18px;
  padding: 0 18px;
  outline: none;
  transition: all 0.2s ease;
  box-sizing: border-box;
}

.password-input::placeholder {
  color: rgba(255, 255, 255, 0.4);
}

.password-input:focus {
  border-color: #32b490;
  background: rgba(255, 255, 255, 0.15);
  box-shadow: 0 0 0 3px rgba(50, 180, 144, 0.1);
}

.account-section {
  gap: 10px;
}

.deactivate-button {
  width: fit-content;
  background: transparent;
  border: none;
  padding: 0;
  color: #d0703a;
  font-size: 18px;
  font-weight: 600;
  cursor: pointer;
  text-decoration: underline;
  transition: all 0.2s ease;
}

.deactivate-button:hover {
  color: #e08850;
  transform: translateX(5px);
}

.buttons-row {
  margin-top: auto;
  display: flex;
  justify-content: flex-end;
  gap: 18px;
  padding-top: 20px;
}

.cancel-button,
.save-button {
  min-width: 160px;
  height: 54px;
  border: none;
  border-radius: 30px;
  font-size: 22px;
  font-weight: 900;
  color: white;
  cursor: pointer;
  transition: all 0.25s ease;
}

.cancel-button {
  background: rgba(1, 59, 43, 0.8);
  border: 1px solid rgba(50, 180, 144, 0.3);
}

.cancel-button:hover {
  background: rgba(1, 45, 33, 0.95);
  transform: translateY(-3px);
  border-color: #32b490;
}

.save-button {
  background: linear-gradient(135deg, #32b490 0%, #046a4e 100%);
}

.save-button:hover {
  transform: translateY(-3px);
  background: linear-gradient(135deg, #55b49a 0%, #32b490 100%);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
}

.cancel-button:active,
.save-button:active {
  transform: scale(0.97);
}

/* ========== АДАПТИВНОСТЬ ========== */
@media (max-width: 1024px) {
  .settings-content {
    padding: 10px 40px 40px;
  }
  .settings-card {
    padding: 30px;
    gap: 25px;
  }
  .settings-left {
    width: 320px;
  }
  .avatar {
    width: 230px;
  }
  .action-button {
    width: 240px;
    font-size: 18px;
  }
}

@media (max-width: 900px) {
  .settings-card {
    flex-direction: column;
    align-items: center;
  }
  .settings-left {
    width: 100%;
    max-width: 350px;
  }
  .settings-divider {
    width: 80%;
    height: 4px;
    margin: 10px 0;
  }
  .settings-right {
    width: 100%;
  }
  .buttons-row {
    justify-content: center;
  }
  .password-input {
    max-width: 100%;
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
  .settings-content {
    padding: 10px 20px 30px;
  }
  .welcome-text {
    font-size: 24px;
  }
  .settings-card {
    padding: 25px;
    border-radius: 36px;
  }
  .avatar {
    width: 180px;
  }
  .avatar-block {
    margin-bottom: 35px;
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
    font-size: 24px;
  }
  .private-label {
    font-size: 18px;
  }
  .private-checkbox {
    width: 22px;
    height: 22px;
  }
  .field-label {
    font-size: 18px;
  }
  .password-input {
    height: 48px;
    font-size: 16px;
    padding: 0 15px;
  }
  .deactivate-button {
    font-size: 16px;
  }
  .cancel-button,
  .save-button {
    min-width: 140px;
    height: 48px;
    font-size: 20px;
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
  .settings-content {
    padding: 10px 15px 20px;
  }
  .settings-card {
    padding: 20px;
    border-radius: 28px;
  }
  .avatar {
    width: 150px;
  }
  .action-button {
    width: 200px;
    font-size: 14px;
    min-height: 45px;
    padding: 10px 12px;
  }
  .section-title {
    font-size: 20px;
  }
  .private-label {
    font-size: 16px;
  }
  .field-label {
    font-size: 16px;
  }
  .password-input {
    height: 44px;
    font-size: 15px;
    padding: 0 12px;
  }
  .deactivate-button {
    font-size: 14px;
  }
  .cancel-button,
  .save-button {
    min-width: 120px;
    height: 44px;
    font-size: 18px;
  }
  .buttons-row {
    gap: 12px;
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
  .section-title {
    font-size: 18px;
  }
  .private-label {
    font-size: 14px;
  }
  .field-label {
    font-size: 14px;
  }
  .cancel-button,
  .save-button {
    min-width: 110px;
    height: 40px;
    font-size: 16px;
  }
}
</style>