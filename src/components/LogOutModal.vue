<template>
  <div class="modal-backdrop" @click="$emit('close')">
    <div class="modal" @click.stop>
      <button class="button-close" @click="$emit('close')">×</button>

      <h2 class="modal-title">уже уходите?</h2>

      <p class="modal-text">
        не переживайте за данные.
        <br />
        мы их сохранили.
      </p>

      <div class="buttons-row">
        <button class="button-submit button-stay" @click="$emit('close')">
          остаться
        </button>

        <button class="button-submit button-exit" @click="$emit('confirm-logout')">
          выйти
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { gsap } from 'gsap'
import { onMounted } from 'vue';
defineEmits(['close', 'confirm-logout'])

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
  background-color: rgba(0, 0, 0, 0.42);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  backdrop-filter: blur(4px);
  -webkit-backdrop-filter: blur(4px);
}

.modal {
  width: 580px;
  height: auto;
  background-color: #046A4D;
  border-radius: 28px;
  position: relative;
  box-shadow: 0 10px 28px rgba(0, 0, 0, 0.2);
  padding: 36px 28px 26px;
  display: flex;
  flex-direction: column;
  align-items: center;
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

.modal-title {
  margin: 18px 0 18px;
  font-size: 48px;
  font-weight: 900;
  color: #fff;
  text-align: center;
}

.modal-text {
  margin: 0 0 24px;
  font-size: 24px;
  font-weight: 400;
  color: #fff;
  text-align: center;
  line-height: 1.3;
}

.buttons-row {
  display: flex;
  gap: 14px;
  justify-content: center;
  align-items: center;
}

.button-submit {
  min-width: 160px;
  height: 48px;
  border: none;
  border-radius: 22px;
  font-size: 24px;
  font-weight: 900;
  color: white;
  cursor: pointer;
  transition: transform 0.25s ease, background-color 0.25s ease, box-shadow 0.25s ease;
}

.button-stay {
  background-color: #013f2d;
}

.button-exit {
  background-color: #165040;
}

.button-submit:hover {
  transform: translateY(-3px) scale(1.02);
  box-shadow: 0 10px 22px rgba(0, 0, 0, 0.18);
}

.button-stay:hover {
  background-color: #012216;
}

.button-exit:hover {
  background-color: #012216;
}

.button-submit:active {
  transform: scale(0.97);
}
</style>