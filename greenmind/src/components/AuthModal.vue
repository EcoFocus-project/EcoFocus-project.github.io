<template>
        <div class="modal-backdrop" @click="$emit('close')">
            <div class="modal" :class="`modal-${mode}`" @click.stop>
                <button class="button-close" @click="$emit('close')">✖</button>

            <LoginForm
                v-if="mode === 'login'"
                @switch-mode="$emit('switch-mode', $event)"
                @success-login="$emit('success-login')"
            />

            <RegisterForm
                v-else="mode ==='register'"
                @switch-mode="$emit('switch-mode', $event)"
                @success-login="$emit('success-login')"
            />
        </div>
    </div>
</template>

<script setup>
import { gsap } from 'gsap'
import { onMounted } from 'vue';
import LoginForm from './LoginForm.vue';
import RegisterForm from './RegisterForm.vue';

defineProps({
    mode: {
        type: String,
        required: true,
    },
})

defineEmits(['close', 'switch-mode', 'success-login'])

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

<style scoped>
.modal-backdrop {
    position: fixed;
    inset: 0;
    background-color: rgba(0,0,0,0.45);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(100px);
}

.modal {
    width: 580px;
    background-color: #046A4D;
    border-radius: 30px;
    display: flex;
    flex-direction: column;
    position: relative;
    box-shadow: 0 20px 40px rgba(0,0,0,0.25);
    padding: 32px 54px 36px;
}

.modal-login {
    min-height: 550px;
}

.modal-register {
    min-height: 790px;
}

.button-close {
    background-color: #014532;
    color: white;
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

</style>