<template>
  <div class="modal-backdrop" @click="$emit('close')">
    <div class="modal" @click.stop>
      <button class="button-close" @click="$emit('close')">×</button>

      <h2 class="modal-title">вы уверены?</h2>

      <p class="modal-text">
        данное действие нельзя
        <br />
        отменить. вы потеряете весь
        <br />
        прогресс!
      </p>

      <div class="buttons-row">
        <button class="delete-button" @click="$emit('confirm-deactivate')">
          удалить аккаунт
        </button>

        <button class="cancel-button" @click="$emit('close')">
          отмена
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { gsap } from 'gsap'
import { onMounted } from 'vue';
defineEmits(['close', 'confirm-deactivate'])

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
  height: 330px;
  width: 620px;
  background-color: #046A4D;
  border-radius: 28px;
  position: relative;
  box-shadow: 0 10px 28px rgba(0, 0, 0, 0.22);
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
  margin: 10px 0 18px;
  font-size: 34px;
  font-weight: 900;
  color: #daf1de;
  text-align: center;
}

.modal-text {
  font-size: 26px;
  font-weight: 400;
  color: #daf1de;
  text-align: center;
  line-height: 1.15;
}

.buttons-row {
  display: flex;
  gap: 14px;
  justify-content: center;
  align-items: center;
}

.delete-button,
.cancel-button {
  min-width: 210px;
  height: 48px;
  border: none;
  border-radius: 18px;
  font-size: 22px;
  font-weight: 900;
  color: white;
  cursor: pointer;
}

.delete-button {
  background-color: #b42924;
}

.cancel-button {
  background-color: #014532;
}

.delete-button:hover,
.cancel-button:hover {
  opacity: 0.92;
}

.delete-button:active,
.cancel-button:active {
  transform: scale(0.98);
}
</style>