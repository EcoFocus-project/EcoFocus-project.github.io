<template>
    <div class="modal-content">
        <img :src="logoImg" alt="логотип" class="logo-image" />

        <div class="form-block">
            <p class="placeholder-input">email</p>
            <input class="modal-input" type="text">
            <p class="placeholder-input">пароль</p>
            <input class="modal-input" type='password'>
        </div>
        <div class="action-block">
            <button type="button" class="button-submit" @click="$emit('success-login')">войти</button>
            <a href="#" class="forgot-password">забыл пароль</a>
            <p class="to-registration">еще нет аккаунта?
                <button class="switch-link" @click="$emit('switch-mode', 'register')">зарегистрировать</button>
            </p>
        </div>
    </div>          
</template>

<script setup>
import logoImg from '../assets/images/logo.png'
defineEmits(['switch-mode', 'success-login'])
</script>

<style scoped>
.modal-content {
    width: 100%;
    max-width: 450px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
}

.form-block {
    display: flex;
    flex-direction: column;
    margin-bottom: 22px;
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
    border-radius: 14px;
    border: none;
    background-color: #c1d1ce;
    padding: 0 16px;
    outline: none;
    font-size: 18px;
    margin-bottom: 14px;
}

.button-submit {
    width: 150px;
    height: 48px;
    font-size: 28px;
    font-weight: 900;
    background-color: #014532;
    color: #fff;
    border: none;
    border-radius: 18px;
    cursor: pointer;
    margin-bottom: 10px;
    transition: background-color 0.25s ease, box-shadow 0.25s ease;
}

.button-submit:hover {
  background-color: #013f2d;
  color: #D1D0D1;
  box-shadow: 0 12px 24px rgba(0,0,0,0.25);
}

.button-submit:active {
  transform: scale(0.97);
  background-color: #014532;
}

.forgot-password {
    text-decoration: none;
    color: #fff;
    font-weight: 700;
    font-size: 16px;
    text-align: center;
    display: block;
    margin-bottom: 26px;
}

.forgot-password:hover {
    color: #D1D0D1;
}

.to-registration {
    color: #fff;
    font-weight: 300;
    font-size: 18px;
    text-align: center;
    margin: 0;
    user-select: none;
    line-height: 1.4;
}

.switch-link {
    text-decoration: underline;
    background-color: transparent;
    border: none;
    padding: 0;
    margin-left: 4px;
    color: #fff;
    font-weight: 500;
    font-size: 18px;
    cursor: pointer;
    font-family: inherit;
}

.switch-link:hover {
    color: #D1D0D1;
}
</style>