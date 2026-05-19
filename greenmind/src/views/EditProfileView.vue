<template>
  <div class="page">
    <div class="header">
      <div class="header-left">
        <img :src="logoImg" alt="логотип" class="logo" />
        <p class="welcome-text">давайте настроим ваш профиль</p>
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

    <div class="edit-content">
      

      <div class="edit-card">
        <div class="edit-left">
          <img :src="avatarImg" alt="аватар" class="main-avatar" />

          <!-- <div class="avatars-grid">
            <button class="avatar-option" v-for="n in 6" :key="n"></button>
          </div> -->
        </div>

        <div class="edit-right">
          <div class="fields-row">
            <div class="field-group wide">
              <label class="field-label">никнейм</label>
              <input v-model="nickname" type="text" class="field-input" />
            </div>

            <div class="field-group narrow">
              <label class="field-label">возраст</label>
              <input v-model="age" type="text" class="field-input" />
            </div>
          </div>

          <div class="fields-row">
            <div class="field-group wide">
              <label class="field-label">страна</label>
              <input v-model="country" type="text" class="field-input" />
            </div>

            <div class="field-group narrow">
              <label class="field-label">пол</label>
              <input v-model="gender" type="text" class="field-input" />
            </div>
          </div>

          <div class="about-group">
            <label class="field-label">о себе</label>
            <textarea v-model="about" class="about-textarea"></textarea>
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

defineEmits(['open-profile', 'go-home'])

const nickname = ref('')
const age = ref('')
const country = ref('')
const gender = ref('')
const about = ref('')

const saveProfile = () => {
  console.log('profile saved')
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

.edit-content {
  padding: 10px 40px 24px;
}

.welcome-text {
  margin: 10px 0 26px;
  font-size: 36px;
  font-weight: 400;
  color: white;
  user-select: none;
}

.edit-card {
  max-width: 1120px;
  min-height: 620px;
  margin: 0 auto;
  background-color: #046A4D;
  border-radius: 48px;
  padding: 36px 40px;
  display: flex;
  gap: 40px;
}

.edit-left {
  width: 230px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 24px;
}

.main-avatar {
  width: 200px;
  height: auto;
  flex-shrink: 0;
}

/* .avatars-grid {
  width: 150px;
  background-color: #c8c8c8;
  border-radius: 14px;
  padding: 14px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
} */

.edit-right {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.fields-row {
  display: flex;
  gap: 26px;
  margin-bottom: 18px;
}

.field-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.field-group.wide {
  width: 420px;
}

.field-group.narrow {
  width: 160px;
}

.field-label {
  font-size: 20px;
  font-weight: 400;
  color: #ffffff;
}

.field-input {
  width: 100%;
  height: 50px;
  border: none;
  border-radius: 12px;
  background-color: #c1d1ce;
  color: #4a4a4a;
  font-size: 20px;
  padding: 0 14px;
  outline: none;
}

.about-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
  margin-top: 2px;
}

.about-textarea {
  width: 100%;
  max-width: 760px;
  height: 170px;
  border: none;
  border-radius: 14px;
  background-color: #c1d1ce;
  color: #4a4a4a;
  font-size: 20px;
  padding: 14px;
  outline: none;
  resize: none;
}

.buttons-row {
  margin-top: auto;
  display: flex;
  justify-content: flex-end;
  gap: 16px;
  padding-top: 28px;
}

.cancel-button,
.save-button {
  min-width: 170px;
  height: 54px;
  border: none;
  border-radius: 18px;
  font-size: 24px;
  font-weight: 900;
  color: white;
  cursor: pointer;
}

.cancel-button {
  background-color: #165040;
}

.save-button {
  background-color: #013f2d;
}

.cancel-button:hover,
.save-button:hover,
.nav-button:hover,
.avatar-option:hover {
  opacity: 0.92;
}

.cancel-button:active,
.save-button:active,
.nav-button:active,
.avatar-option:active {
  transform: scale(0.98);
}
</style>