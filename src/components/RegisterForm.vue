<template>
  <div class="modal-content">
    <h1 class="header">регистрация</h1>
    <img :src="avatarImg" alt="аватар" class="avatar" />

    <div class="sex-choice-block">
      <p class="placeholder-input">пол</p>
      <div class="sex-options">
        <label for="sexChoice1" class="sex-option">
          <input type="radio" id="sexChoice1" name="sex" value="female" />
          <span class="custom-radio"></span>
          <span class="option-text">женский</span>
        </label>

        <label for="sexChoice2" class="sex-option">
          <input type="radio" id="sexChoice2" name="sex" value="male" />
          <span class="custom-radio"></span>
          <span class="option-text">мужской</span>
        </label>

        <label for="sexChoice3" class="sex-option">
          <input type="radio" id="sexChoice3" name="sex" value="no" />
          <span class="custom-radio"></span>
          <span class="option-text">не указывать</span>
        </label>
      </div>
    </div>

    <div class="form-block">
      <p class="placeholder-input">никнейм</p>
      <input type="text" class="modal-input" placeholder="Придумайте никнейм">

      <p class="placeholder-input">email</p>
      <input type="text" class="modal-input" placeholder="Введите email">

      <p class="placeholder-input">пароль</p>
      <div class="password-wrapper">
        <input :type="showPassword ? 'text' : 'password'" class="modal-input" placeholder="Придумайте пароль">
        <button type="button" class="password-toggle" @click="showPassword = !showPassword">
          {{ showPassword ? '🙈' : '👁️' }}
        </button>
      </div>
    </div>

    <div class="action-block">
      <button class="button-submit">
        <span class="submit-text">продолжить</span>
        <span class="submit-icon">→</span>
      </button>
      <p class="to-login">уже есть аккаунт?
        <button class="switch-link" @click="$emit('switch-mode', 'login')">войти</button>
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import avatarImg from '../assets/images/avatar.png'

defineEmits(['switch-mode'])

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

.header {
  text-align: center;
  color: #fff;
  font-size: 42px;
  font-weight: 900;
  margin: 0 0 10px 0;
  user-select: none;
}

.avatar {
  height: auto;
  width: 100px;
  margin: 0 auto 15px;
  display: block;
  animation: gentleFloat 3s ease-in-out infinite;
}

@keyframes gentleFloat {
  0%, 100% { transform: translateY(0px); }
  50% { transform: translateY(-6px); }
}

.sex-choice-block {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin-bottom: 5px;
}

.sex-options {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  align-items: center;
}

.sex-option {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  user-select: none;
  position: relative;
}

.sex-option input[type='radio'] {
  position: absolute;
  opacity: 0;
  pointer-events: none;
}

.custom-radio {
  width: 18px;
  height: 18px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.1);
  border: 2px solid rgba(50, 180, 144, 0.5);
  display: inline-block;
  position: relative;
  transition: all 0.2s ease;
  flex-shrink: 0;
}

.option-text {
  color: white;
  font-size: 16px;
  font-weight: 400;
}

.sex-option input[type='radio']:checked + .custom-radio {
  border-color: #32b490;
}

.sex-option input[type='radio']:checked + .custom-radio::after {
  content: '';
  position: absolute;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background-color: #32b490;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.form-block {
  display: flex;
  flex-direction: column;
  width: 100%;
}

.placeholder-input {
  color: white;
  font-weight: 400;
  user-select: none;
  margin: 0 0 8px 0;
  font-size: 16px;
  letter-spacing: 0.5px;
}

.modal-input {
  width: 100%;
  height: 50px;
  border-radius: 12px;
  border: 1px solid rgba(50, 180, 144, 0.3);
  background: rgba(255, 255, 255, 0.1);
  padding: 0 16px;
  outline: none;
  font-size: 16px;
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
  width: 220px;
  height: 52px;
  font-size: 20px;
  font-weight: 900;
  background: linear-gradient(135deg, #32b490 0%, #046a4e 100%);
  color: #fff;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  margin: 10px auto 15px;
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

.to-login {
  color: rgba(255, 255, 255, 0.7);
  font-weight: 300;
  font-size: 14px;
  text-align: center;
  margin: 0 0 5px 0;
  line-height: 1.4;
}

.switch-link {
  background-color: transparent;
  border: none;
  padding: 0;
  margin-left: 4px;
  color: #32b490;
  font-weight: 500;
  font-size: 14px;
  cursor: pointer;
  font-family: inherit;
  text-decoration: underline;
  transition: all 0.2s ease;
}

.switch-link:hover {
  color: #55b49a;
}

@media (max-width: 640px) {
  .header { font-size: 34px; }
  .avatar { width: 80px; margin-bottom: 10px; }
  .placeholder-input { font-size: 14px; margin-bottom: 6px; }
  .modal-input { height: 44px; font-size: 15px; margin-bottom: 10px; }
  .password-wrapper .modal-input { margin-bottom: 10px; }
  .password-toggle { bottom: 18px; font-size: 18px; }
  .button-submit { width: 200px; height: 48px; font-size: 18px; }
  .submit-icon { font-size: 18px; right: 25px; }
  .option-text { font-size: 14px; }
  .sex-options { gap: 15px; }
  .to-login, .switch-link { font-size: 13px; }
}
</style>