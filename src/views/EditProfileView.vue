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
          <div class="avatar-container">
            <div class="main-avatar">{{ selectedAvatarEmoji }}</div>
          </div>
          
          <div class="avatars-grid">
            <button
              v-for="avatar in avatars"
              :key="avatar.id"
              class="avatar-option"
              :class="{ 'avatar-selected': selectedAvatar === avatar.id }"
              @click="selectAvatar(avatar.id)"
            >
              <span class="avatar-emoji">{{ avatar.emoji }}</span>
            </button>
          </div>
        </div>

        <div class="edit-right">
          <div class="field-group">
            <label class="field-label">никнейм</label>
            <input v-model="nickname" type="text" class="field-input" placeholder="Ваш никнейм" />
          </div>

          <div class="field-group">
            <label class="field-label">страна</label>
            <input v-model="country" type="text" class="field-input" placeholder="Страна проживания" />
          </div>

          <div class="field-group">
            <label class="field-label">о себе</label>
            <textarea v-model="about" class="about-textarea" placeholder="Расскажите о себе..."></textarea>
          </div>

          <div class="buttons-row">
            <button class="cancel-button" @click="$emit('open-profile')">
              отменить
            </button>
            <button class="save-button" @click="saveProfile">
              сохранить
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import backImg from '../assets/images/back.png'
import profileImg from '../assets/images/profile.png'
import logoImg from '../assets/images/logo.png'

const emit = defineEmits(['open-profile', 'go-home'])

const nickname = ref('')
const country = ref('')
const about = ref('')
const selectedAvatar = ref(1)

const avatars = [
  { id: 1, emoji: '👨‍🚀', name: 'Космонавт' },
  { id: 2, emoji: '🧜‍♀️', name: 'Русалка' },
  { id: 3, emoji: '🐬', name: 'Дельфин' },
  { id: 4, emoji: '🐢', name: 'Черепаха' },
  { id: 5, emoji: '🐙', name: 'Осьминог' },
  { id: 6, emoji: '🐋', name: 'Кит' },
  { id: 7, emoji: '⭐', name: 'Морская звезда' },
  { id: 8, emoji: '🐠', name: 'Рыбка' }
]

const selectedAvatarEmoji = computed(() => {
  const avatar = avatars.find(a => a.id === selectedAvatar.value)
  return avatar ? avatar.emoji : '👤'
})

const selectAvatar = (id) => {
  selectedAvatar.value = id
}

const saveProfile = () => {
  console.log('profile saved', {
    nickname: nickname.value,
    country: country.value,
    about: about.value,
    avatar: selectedAvatar.value
  })
  emit('open-profile')
}
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

.edit-content {
  padding: 10px 40px 40px;
  position: relative;
  z-index: 2;
}

.welcome-text {
  font-size: 36px;
  font-weight: 400;
  color: white;
  user-select: none;
}

.edit-card {
  max-width: 1200px;
  margin: 0 auto;
  background: rgba(4, 106, 77, 0.8);
  backdrop-filter: blur(5px);
  border-radius: 48px;
  padding: 40px 50px;
  display: flex;
  gap: 60px;
  border: 1px solid rgba(50, 180, 144, 0.3);
}

.edit-left {
  width: 280px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 24px;
}

.avatar-container {
  width: 200px;
  height: 200px;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 3px solid rgba(50, 180, 144, 0.5);
}

.main-avatar {
  font-size: 100px;
  animation: gentleFloat 3s ease-in-out infinite;
}

/* Сетка аватарок */
.avatars-grid {
  width: 100%;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 12px;
  background: rgba(1, 69, 48, 0.6);
  backdrop-filter: blur(5px);
  border-radius: 20px;
  padding: 15px;
  border: 1px solid rgba(50, 180, 144, 0.2);
}

.avatar-option {
  background: rgba(255, 255, 255, 0.1);
  border: 2px solid rgba(50, 180, 144, 0.3);
  border-radius: 16px;
  padding: 10px 5px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.2s ease;
}

.avatar-option:hover {
  background: rgba(50, 180, 144, 0.2);
  transform: translateY(-3px);
  border-color: #55b49a;
}

.avatar-selected {
  border-color: #32b490;
  background: rgba(50, 180, 144, 0.3);
  box-shadow: 0 0 0 2px rgba(50, 180, 144, 0.3);
}

.avatar-emoji {
  font-size: 28px;
}

.edit-right {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.field-group {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.field-label {
  font-size: 20px;
  font-weight: 500;
  color: #ffffff;
  letter-spacing: 0.5px;
}

.field-input {
  width: 100%;
  height: 55px;
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

.field-input::placeholder {
  color: rgba(255, 255, 255, 0.4);
}

.field-input:focus {
  border-color: #32b490;
  background: rgba(255, 255, 255, 0.15);
  box-shadow: 0 0 0 3px rgba(50, 180, 144, 0.1);
}

.about-textarea {
  width: 100%;
  min-height: 140px;
  border: 1px solid rgba(50, 180, 144, 0.3);
  border-radius: 14px;
  background: rgba(255, 255, 255, 0.1);
  color: white;
  font-size: 18px;
  padding: 14px 18px;
  outline: none;
  resize: vertical;
  transition: all 0.2s ease;
  font-family: inherit;
  box-sizing: border-box;
}

.about-textarea::placeholder {
  color: rgba(255, 255, 255, 0.4);
}

.about-textarea:focus {
  border-color: #32b490;
  background: rgba(255, 255, 255, 0.15);
  box-shadow: 0 0 0 3px rgba(50, 180, 144, 0.1);
}

.buttons-row {
  display: flex;
  justify-content: flex-end;
  gap: 20px;
  margin-top: 10px;
}

.cancel-button,
.save-button {
  min-width: 160px;
  height: 55px;
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
  .edit-card {
    padding: 35px 40px;
    gap: 40px;
  }
}

@media (max-width: 900px) {
  .edit-card {
    flex-direction: column;
    align-items: center;
    padding: 35px;
  }
  
  .edit-left {
    width: 100%;
    max-width: 300px;
  }
  
  .edit-right {
    width: 100%;
  }
  
  .buttons-row {
    justify-content: center;
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
  
  .edit-content {
    padding: 10px 20px 30px;
  }
  
  .welcome-text {
    font-size: 24px;
  }
  
  .edit-card {
    padding: 25px;
    border-radius: 36px;
  }
  
  .avatar-container {
    width: 160px;
    height: 160px;
  }
  
  .main-avatar {
    font-size: 80px;
  }
  
  .avatar-emoji {
    font-size: 22px;
  }
  
  .avatars-grid {
    gap: 10px;
    padding: 12px;
  }
  
  .field-label {
    font-size: 18px;
  }
  
  .field-input {
    height: 48px;
    font-size: 16px;
    padding: 0 15px;
  }
  
  .about-textarea {
    font-size: 16px;
    min-height: 120px;
    padding: 12px 15px;
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
  
  .edit-content {
    padding: 10px 15px 20px;
  }
  
  .edit-card {
    padding: 20px;
    border-radius: 28px;
  }
  
  .avatar-container {
    width: 130px;
    height: 130px;
  }
  
  .main-avatar {
    font-size: 65px;
  }
  
  .avatars-grid {
    grid-template-columns: repeat(4, 1fr);
    gap: 8px;
    padding: 10px;
  }
  
  .avatar-emoji {
    font-size: 18px;
  }
  
  .field-label {
    font-size: 16px;
  }
  
  .field-input {
    height: 44px;
    font-size: 15px;
    padding: 0 12px;
  }
  
  .about-textarea {
    font-size: 15px;
    min-height: 100px;
    padding: 10px 12px;
  }
  
  .cancel-button,
  .save-button {
    min-width: 120px;
    height: 44px;
    font-size: 18px;
  }
  
  .buttons-row {
    gap: 15px;
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
  
  .avatar-container {
    width: 110px;
    height: 110px;
  }
  
  .main-avatar {
    font-size: 55px;
  }
  
  .avatar-emoji {
    font-size: 16px;
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