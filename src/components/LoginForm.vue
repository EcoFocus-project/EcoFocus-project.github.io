<template>
  <div class="modal-content">
    <img :src="logoImg" alt="логотип" class="logo-image" />

    <div class="form-block">
      <p class="placeholder-input">email</p>
      <input class="modal-input" type="text" placeholder="Введите email">

      <p class="placeholder-input">пароль</p>
      <div class="password-wrapper">
        <input :type="showPassword ? 'text' : 'password'" class="modal-input" placeholder="Введите пароль">
        <button type="button" class="password-toggle" @click="showPassword = !showPassword">
          {{ showPassword ? '🙈' : '👁️' }}
        </button>
      </div>
    </div>

    <div class="action-block">
      <button type="button" class="button-submit" @click="$emit('success-login')">
        <span class="submit-text">войти</span>
        <span class="submit-icon">→</span>
      </button>
      <a href="#" class="forgot-password">забыл пароль</a>
      <p class="to-registration">еще нет аккаунта?
        <button class="switch-link" @click="$emit('switch-mode', 'register')">зарегистрироваться</button>
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import logoImg from '../assets/images/logo.png'

defineEmits(['switch-mode', 'success-login'])

const showPassword = ref(false)
</script>

<style scoped>
.modal-content {
  width: 100%;
  max-width: 450px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 0 8px 16px 8px;
  min-height: 0;
}

.form-block {
  display: flex;
  flex-direction: column;
}

.action-block {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.logo-image {
  width: 120px;
  height: auto;
  margin: 0 auto 20px;
  object-fit: contain;
  display: block;
  animation: gentleFloat 3s ease-in-out infinite;
}

@keyframes gentleFloat {
  0%, 100% { transform: translateY(0px); }
  50% { transform: translateY(-6px); }
}

.placeholder-input {
  color: white;
  font-weight: 300;
  user-select: none;
  margin: 0 0 8px 0;
  font-size: 22px;
}

.modal-input {
  width: 100%;
  height: 50px;
  border-radius: 12px;
  border: 1px solid rgba(50, 180, 144, 0.3);
  background: rgba(255, 255, 255, 0.1);
  padding: 0 16px;
  outline: none;
  font-size: 18px;
  margin-bottom: 14px;
  color: white;
  transition: all 0.2s ease;
  box-sizing: border-box;
}

.modal-input::placeholder {
  color: rgba(255, 255, 255, 0.4);
}

.modal-input:focus {
  border-color: #32b490;
  background: rgba(255, 255, 255, 0.15);
  box-shadow: 0 0 0 3px rgba(50, 180, 144, 0.1);
}

.password-wrapper {
  position: relative;
  width: 100%;
}

.password-wrapper .modal-input {
  margin-bottom: 14px;
  padding-right: 45px;
}

.password-toggle {
  position: absolute;
  right: 12px;
  bottom: 22px;
  background: transparent;
  border: none;
  cursor: pointer;
  font-size: 20px;
  padding: 0;
  transition: all 0.2s ease;
  color: rgba(255, 255, 255, 0.6);
}

.password-toggle:hover {
  color: #32b490;
  transform: scale(1.1);
}

.button-submit {
  width: 180px;
  height: 52px;
  font-size: 22px;
  font-weight: 900;
  background: linear-gradient(135deg, #32b490 0%, #046a4e 100%);
  color: #fff;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  margin-bottom: 15px;
  transition: all 0.2s ease;
  position: relative;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}

.submit-text {
  transition: transform 0.2s ease;
}

.submit-icon {
  position: absolute;
  font-size: 20px;
  opacity: 0;
  transform: translateX(-10px);
  transition: all 0.2s ease;
  right: 30px;
}

.button-submit::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: left 0.4s ease;
}

.button-submit:hover::before {
  left: 100%;
}

.button-submit:hover {
  transform: translateY(-2px);
  background: linear-gradient(135deg, #55b49a 0%, #32b490 100%);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
}

.button-submit:hover .submit-text {
  transform: translateX(-10px);
}

.button-submit:hover .submit-icon {
  opacity: 1;
  transform: translateX(0);
}

.forgot-password {
  text-decoration: none;
  color: rgba(255, 255, 255, 0.7);
  font-weight: 400;
  font-size: 14px;
  text-align: center;
  display: block;
  margin-bottom: 20px;
  transition: all 0.2s ease;
}

.forgot-password:hover {
  color: #32b490;
}

.to-registration {
  color: rgba(255, 255, 255, 0.7);
  font-weight: 300;
  font-size: 14px;
  text-align: center;
  margin: 0;
  line-height: 1.4;
}

.switch-link {
  text-decoration: underline;
  background-color: transparent;
  border: none;
  padding: 0;
  margin-left: 4px;
  color: #32b490;
  font-weight: 500;
  font-size: 14px;
  cursor: pointer;
  font-family: inherit;
  transition: all 0.2s ease;
}

.switch-link:hover {
  color: #55b49a;
}

@media (max-width: 640px) {
  .logo-image { width: 80px; margin-bottom: 16px; }
  .placeholder-input { font-size: 18px; margin-bottom: 6px; }
  .modal-input { height: 44px; font-size: 16px; margin-bottom: 10px; }
  .password-wrapper .modal-input { margin-bottom: 10px; }
  .password-toggle { bottom: 18px; font-size: 18px; }
  .button-submit { width: 160px; height: 48px; font-size: 20px; }
  .submit-icon { font-size: 18px; right: 25px; }
  .to-registration, .switch-link { font-size: 13px; }
  .forgot-password { font-size: 13px; }
}
</style>