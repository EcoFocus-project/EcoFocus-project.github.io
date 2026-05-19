<template>
  <div class="page">
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
              выйти <br />
              из аккаунта
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

            <p class="field-label">смена пароля</p>

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
import avatarImg from '../assets/images/avatar.png'
import backImg from '../assets/images/back.png'
import profileImg from '../assets/images/profile.png'
import logoImg from '../assets/images/logo.png'
import { ref } from 'vue'

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

const saveSettings = () => {
  console.log('settings saved', 'open-profile', 'open-deactive', 'go-home')
}
</script>

<style scoped>
.page {
  min-height: 100vh;
  background-image: linear-gradient(to bottom, #046A4D, #023522);
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 30px;
  user-select: none;
}

.header-left {
    display: flex;
    flex-direction: row;
    gap: 10px;
    align-items: center;
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
  min-width: 230px;
  height: 60px;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  background-color: #0544263f;
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
  background-color: #013b2b;
  box-shadow: 0 12px 24px rgba(0,0,0,0.25);
}

.nav-button:active {
  transform: scale(0.97);
  background-color: #013b2b;
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
  padding: 10px 80px 10px;
}

.welcome-text {
  font-size: 36px;
  font-weight: 400;
  color: white;
  user-select: none;
}

.settings-card {
  background-color: #046A4D;
  border-radius: 48px;
  padding: 36px 40px;
  display: flex;
  gap: 28px;
  min-height: 560px;
  margin: 0 auto;
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
    width: 300px;
    height: auto;
}

.profile-actions {
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 34px;
  align-items: center;
}

.action-button {
  width: 270px;
  min-height: 60px;
  border: none;
  border-radius: 20px;
  background-color: #013b2b;
  color: white;
  font-size: 24px;
  font-weight: 900;
  cursor: pointer;
  padding: 0 16px;
  line-height: 1.1;
}

.action-button:hover {
    background-color: #012216;
    box-shadow: 0 12px 24px rgba(0,0,0,0.25);
}

.current-button {
  background-color: #013b2b;
}

.settings-divider {
  width: 4px;
  background-color: #013b2b;
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
  font-size: 32px;
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
  width: 28px;
  height: 28px;
  accent-color: #014532;
  cursor: pointer;
}

.private-label {
  font-size: 24px;
  font-weight: 400;
  color: #ffffff;
}

.field-label {
  margin: 0;
  font-size: 24px;
  font-weight: 400;
  color: #ffffff;
}

.password-input {
  width: 460px;
  max-width: 100%;
  height: 52px;
  border: none;
  border-radius: 12px;
  background-color: #c8c8c8;
  color: #4a4a4a;
  font-size: 24px;
  padding: 0 16px;
  outline: none;
}

.password-input::placeholder {
  color: #666666;
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
  font-size: 20px;
  font-weight: 600;
  cursor: pointer;
  text-decoration: underline;
}

.deactivate-button:hover {
  text-decoration: underline;
}

.buttons-row {
  margin-top: auto;
  display: flex;
  justify-content: flex-end;
  gap: 18px;
}

.cancel-button,
.save-button {
  min-width: 180px;
  height: 54px;
  border: none;
  border-radius: 18px;
  font-size: 24px;
  font-weight: 900;
  color: white;
  cursor: pointer;
}

.cancel-button {
  background-color: #0544263f;
}

.save-button {
  background-color: #04361e94;
}

.cancel-button:hover,
.save-button:hover,
.action-button:hover,
.nav-button:hover,
.avatar-edit-button:hover {
  opacity: 0.92;
}

.cancel-button:active,
.save-button:active,
.action-button:active,
.nav-button:active,
.avatar-edit-button:active {
  transform: scale(0.98);
}
</style>