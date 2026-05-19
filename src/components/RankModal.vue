<template>
  <div class="modal-backdrop" @click="$emit('close')">
    <div class="modal" @click.stop>
      <button class="button-close" @click="$emit('close')">×</button>

      <h2 class="section-title">ваш текущий ранг</h2>

      <div class="current-rank-card">
        <div class="rank-main">
          <div class="rank-left">
            <p class="rank-name">юный эколог</p>

            <div class="xp-block">
              <div class="xp-bar">
              </div>
              <p class="xp-caption">1000 xp до следующего ранга</p>
            </div>
          </div>

          <div class="rank-right">
            <img :src="badgeImg" alt="бейдж" class="rank-badge">
            <p class="xp-text">0/1000 xp</p>
          </div>
        </div>
      </div>

      <h2 class="section-title next-title">следующий ранг</h2>

      <div class="next-rank-list">
        <div class="next-rank-card">
          <div class="next-rank-left">
            <p class="next-rank-name">защитник земли</p>
            <p class="next-rank-range">1000-1500 xp</p>
            <p class="next-rank-caption">n xp до данного ранга</p>
          </div> 
          <img :src="badgeImg2" alt="бейдж" class="next-rank-icon">
        </div>

        <div class="next-rank-card">
          <div class="next-rank-left">
            <p class="next-rank-name">хранитель равновесия</p>
            <p class="next-rank-range">1500-2000 xp</p>
            <p class="next-rank-caption">n xp до данного ранга</p>
          </div>
          <img :src="badgeImg3" alt="бейдж" class="next-rank-icon">
        </div>

        <div class="next-rank-card">
          <div class="next-rank-left">
            <p class="next-rank-name">легенда greenmind</p>
            <p class="next-rank-range">2000+ xp</p>
          </div>
          <img :src="badgeImg4" alt="бейдж" class="next-rank-icon">
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { gsap } from 'gsap'
import { onMounted } from 'vue';
import badgeImg4 from '../assets/images/rank_icon4.png'
import badgeImg3 from '../assets/images/rank_icon3.png'
import badgeImg2 from '../assets/images/rank_icon2.png'
import badgeImg from '../assets/images/rank_icon.png'
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
  min-height: 690px;
  background-color: #046A4D;
  border-radius: 28px;
  position: relative;
  box-shadow: 0 10px 28px rgba(0, 0, 0, 0.2);
  padding: 24px 18px 18px;
  display: flex;
  flex-direction: column;
}

.button-close {
  position: absolute;
  top: 12px;
  right: 12px;
  width: 42px;
  height: 42px;
  border-radius: 50%;
  border: none;
  background-color: #014532;
  color: white;
  font-size: 26px;
  font-weight: 700;
  cursor: pointer;
  line-height: 1;
}

.section-title {
  margin: 18px 0 14px;
  font-size: 32px;
  font-weight: 900;
  color: #ffffff;
}

.next-title {
  margin-top: 18px;
}

.current-rank-card {
  background-color: #014530;
  border-radius: 26px;
  padding: 18px 16px 14px;
}

.rank-main {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 16px;
}

.rank-left {
  flex: 1;
}

.rank-name {
  margin: 0 0 14px;
  font-size: 30px;
  font-weight: 900;
  color: white;
  line-height: 1.1;
}

.rank-badge {
    width: 70px;
    height: auto;
    flex-shrink: 0;
}

.xp-block {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.xp-bar {
  width: 100%;
  height: 10px;
  border-radius: 10px;
  background-color: #55b49a;
  overflow: hidden;
}


.xp-caption {
  margin: 0;
  font-size: 20px;
  font-weight: 300;
  color: #e7e7e7;
  text-align: center;
}

.rank-right {
  width: 120px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  flex-shrink: 0;
}

.rank-icon {
  width: 66px;
  height: auto;
}

.xp-text {
  margin: 0;
  font-size: 18px;
  font-weight: 400;
  color: white;
}

.next-rank-list {
  display: flex;
  flex-direction: column;
  gap: 8px;
}
.next-rank-card {
  background-color: #014530;
  border-radius: 24px;
  padding: 14px 16px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 16px;
}

.next-rank-left {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.next-rank-name {
  margin: 0;
  font-size: 24px;
  font-weight: 900;
  color: white;
  line-height: 1.1;
}

.next-rank-range {
  margin: 0;
  font-size: 22px;
  font-weight: 400;
  color: white;
}

.next-rank-caption {
  margin: 0;
  font-size: 20px;
  font-weight: 300;
  color: #e7e7e7;
}

.next-rank-icon {
  width: 70px;
  height: auto;
  flex-shrink: 0;
}
</style>