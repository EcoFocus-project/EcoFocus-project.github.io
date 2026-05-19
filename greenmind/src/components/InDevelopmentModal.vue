<template>
    <div class="modal-backdrop" @click="$emit('close')">
        <div class="modal" @click.stop>
            <button class="button-close" @click="$emit('close')">✖</button>

            <h1 class="title">ой!</h1>

            <p class="text">данный раздел находится на стадии разработки!</p>
        </div>
    </div>
</template>

<style>
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
        height: 380px;
        background-color: #046A4D;
        border-radius: 30px;
        display: flex;
        flex-direction: column;
        position: relative;
        box-shadow: 0 20px 40px rgba(0,0,0,0.25);
        padding: 26px 34px 30px;
        color: #daf1de;
    }

    .button-close {
        background-color: #014532;
        color: #daf1de;
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

    .title {
        font-size: 48px;
        font-weight: 900;
        text-align: center;
        user-select: none;
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
defineEmits(['close'])

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