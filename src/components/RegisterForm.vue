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
            <input type="text" class="modal-input">
            <p class="placeholder-input">email</p>
            <input type="text" class="modal-input">
            <p class="placeholder-input">пароль</p>
            <input type="password" class="modal-input">
        </div>
        <div class="action-block">
            <button class="button-submit">продолжить</button>
            <p class="to-login">уже есть аккаунт?
                <button class="switch-link" @click="$emit('switch-mode', 'login')">войти</button>
            </p>
        </div>
    </div>
</template>

<script setup>
import avatarImg from '../assets/images/avatar.png'
defineEmits(['switch-mode'])
</script>

<style scoped>

.modal-content {
    width: 100%;
    max-width: 390px;
    margin: 0 auto;
    height: auto;
    display: flex;
    flex-direction: column;
    align-items: center;
    color: #fff;
    font-weight: 100;
    font-size: 20px;
}

.header {
    text-align: center;
    color: #fff;
    font-size: 48px;
    font-weight: 900;
    margin: 24px 0 20px;
    user-select: none;
}

.avatar {
    height: auto;
    width: 140px;
    align-items: center;
    text-align: center;
}

.sex-choice-block {
    width: 100%;
    max-width: 390px;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    margin-bottom: 20px;
}

.sex-options {
    display: flex;
    flex-wrap: wrap;
    gap: 18px;
    align-items: center;
}

.sex-option {
    display: flex;
    align-items: center;
    gap: 6px;
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
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background-color: #014532;
    border: 2px solid #014532;
    display: inline-block;
    position: relative;
    transition: all 0.2s ease;
    flex-shrink: 0;
}

.option-text {
    color: white;
    font-size: 18px;
    font-weight: 300;
}

.sex-option input[type='radio']:checked + .custom-radio::after {
    content: '';
    position: absolute;
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background-color: #55b49a;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}

.form-block {
    display: flex;
    flex-direction: column;
    width: 100%;
    max-width: 390px;
}


.placeholder-input {
    color: #fff;
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
    width: 300px;
    height: 48px;
    font-size: 28px;
    font-weight: 900;
    background-color: #014532;
    color: #fff;
    border: none;
    border-radius: 18px;
    cursor: pointer;
    margin: 20px 10px;
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

.to-login {
    color: #fff;
    font-weight: 300;
    font-size: 18px;
    text-align: center;
    margin: 0;
    user-select: none;
    line-height: 1.4;
}

.switch-link {
    background-color: transparent;
    border: none;
    padding: 0;
    margin-left: 4px;
    color: #daf1de;
    font-weight: 300;
    font-size: 18px;
    cursor: pointer;
    font-family: inherit;
    text-decoration: underline;
}

.switch-link:hover {
    color: #D1D0D1;
}


</style>