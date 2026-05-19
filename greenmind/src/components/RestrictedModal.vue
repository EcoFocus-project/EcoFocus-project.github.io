<template>
    <div class="modal-backdrop" @click="$emit('close')">
        <div class="modal" @click.stop>
            <button class="button-close" @click="$emit('close')">✖</button>

            <h1 class="title">упс..</h1>

            <p class="text">для этого вам необходимо
                <br/>
                иметь аккаунт.
            </p>
            <div class="buttons-block">
                <button class="button-submit button-login" @click="$emit('open-login')">войти</button>
                <button class="button-submit button-register" @click="$emit('open-register')">зарегистрироваться</button>
            </div>
        </div>
    </div>
</template>

<style scoped>
.modal-backdrop {
    position: fixed;
    inset: 0;
    background-color: rgba(0,0,0,0.45);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
    backdrop-filter: blur(3px);
    -webkit-backdrop-filter: blur(3x);
}

.modal {
    width: 580px;
    height: 550px;
    background-color: #046A4D;
    border-radius: 30px;
    display: flex;
    flex-direction: column;
    position: relative;
    box-shadow: 0 20px 40px rgba(0,0,0,0.25);
    padding: 26px 34px 30px;
    color: #fff;
}

.button-close {
    background-color: #014532;
    color: #fff;
    font-weight: 700;
    font-size: 25px;
    top: 18px;
    right: 18px;
    width: 49px;
    height: 49px;
    border-radius: 50%;
    border: none;
    position: absolute;
    cursor: pointer;
}

.button-submit {
    width: 300px;
    height: 63px;
    font-size: 28px;
    font-weight: 900;
    background-color: #014532;
    color: #fff;
    border: none;
    border-radius: 23px;
    cursor: pointer;
    margin-bottom: 30px;
    transition: transform 0.25s ease, background-color 0.25s ease, box-shadow 0.25s ease;
}

.button-submit:hover {
  transform: translateY(-4px) scale(1.03);
  background-color: #013f2d;
  box-shadow: 0 12px 24px rgba(0,0,0,0.25);
}

.button-submit:active {
  transform: scale(0.97);
  background-color: #014532;
}

.button-login {
    width: 272px;
}

.button-register {
    width: 370px;
}

.title {
    font-size: 48px;
    font-weight: 900;
    text-align: center;
    user-select: none;
}

.buttons-block {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 15px;
    gap: 5px;
    width: 100%;
}

.text {
    font-size: 30px;
    font-weight: 400;
    text-align: center;
    margin: 0 0 34px;
}
</style>

<script setup>
import { gsap } from 'gsap'
import { onMounted } from 'vue';

defineEmits(['close', 'open-login', 'open-register'])

const animateModal = () => {
    const tl = gsap.timeline()

    tl.fromTo (
        '.modal-backdrop',
        {
            opacity: 0
        },
        {
            opacity: 1,
            duration: 0.25,
            ease: 'power2.out'
        }
    ).fromTo(
        '.modal',
        {
            opacity: 0,
            scale: 0.85,
            y: -20
        },
        {
            opacity: 1,
            scale: 1,
            y: 0,
            duration: 0.45,
            ease: 'power4.out'
        },
        '-=0.1'
    )
}

onMounted(() => {
    animateModal()
})
</script>