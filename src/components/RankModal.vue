<template>
  <div class="modal-backdrop" @click="$emit('close')">
    <div class="modal" @click.stop>
      <!-- Декоративные элементы -->
      <div class="modal-decor">
        <div class="decor-leaf leaf1">🍃</div>
        <div class="decor-leaf leaf2">🌿</div>
        <div class="decor-leaf leaf3">🍂</div>
        <div class="decor-circle circle1"></div>
        <div class="decor-circle circle2"></div>
        <div class="decor-circle circle3"></div>
      </div>

      <button class="button-close" @click="$emit('close')">✖</button>

      <div class="modal-icon">🏆</div>
      <h2 class="section-title">ваш текущий ранг</h2>

      <div class="current-rank-card">
        <div class="rank-main">
          <div class="rank-left">
            <p class="rank-name">юный эколог</p>

            <div class="xp-block">
              <div class="xp-bar">
                <div class="xp-fill" style="width: 0%"></div>
              </div>
              <p class="xp-caption">1000 xp до следующего ранга</p>
            </div>
          </div>

          <div class="rank-right">
            <img :src="badgeImg" alt="бейдж" class="rank-badge" />
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
          <img :src="badgeImg2" alt="бейдж" class="next-rank-icon" />
        </div>

        <div class="next-rank-card">
          <div class="next-rank-left">
            <p class="next-rank-name">хранитель равновесия</p>
            <p class="next-rank-range">1500-2000 xp</p>
            <p class="next-rank-caption">n xp до данного ранга</p>
          </div>
          <img :src="badgeImg3" alt="бейдж" class="next-rank-icon" />
        </div>

        <div class="next-rank-card">
          <div class="next-rank-left">
            <p class="next-rank-name">легенда greenmind</p>
            <p class="next-rank-range">2000+ xp</p>
          </div>
          <img :src="badgeImg4" alt="бейдж" class="next-rank-icon" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { gsap } from 'gsap'
import { onMounted } from 'vue'
import badgeImg4 from '../assets/images/rank_icon4.png'
import badgeImg3 from '../assets/images/rank_icon3.png'
import badgeImg2 from '../assets/images/rank_icon2.png'
import badgeImg from '../assets/images/rank_icon.png'

defineEmits(['close'])

const animateModal = () => {
  const tl = gsap.timeline()
  tl.fromTo('.modal-backdrop', { opacity: 0 }, { opacity: 1, duration: 0.25, ease: 'power2.out' })
    .fromTo('.modal', { opacity: 0, scale: 0.85, y: -20 }, { opacity: 1, scale: 1, y: 0, duration: 0.45, ease: 'power4.out' }, '-=0.1')
}

onMounted(() => {
  animateModal()
})
</script>

<style scoped>
/* Анимации */
@keyframes floatLeaf {
  0%, 100% { transform: translateY(0px) rotate(0deg); }
  50% { transform: translateY(-12px) rotate(8deg); }
}

@keyframes floatCircle {
  0%, 100% { transform: translate(0, 0); }
  25% { transform: translate(8px, -8px); }
  50% { transform: translate(-4px, 10px); }
  75% { transform: translate(10px, 4px); }
}

@keyframes gentleFloat {
  0%, 100% { transform: translateY(0px); }
  50% { transform: translateY(-6px); }
}

.modal-backdrop {
  position: fixed;
  inset: 0;
  background: linear-gradient(135deg, rgba(4, 106, 78, 0.85) 0%, rgba(1, 69, 50, 0.9) 100%);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  backdrop-filter: blur(15px);
  padding: 20px;
}

.modal {
  width: 100%;
  max-width: 580px;
  background: linear-gradient(135deg, #046a4e, #014532);
  border-radius: 48px;
  position: relative;
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.3), 0 0 0 1px rgba(50, 180, 144, 0.2);
  padding: 40px 35px 45px;
  display: flex;
  flex-direction: column;
  border: 1px solid rgba(50, 180, 144, 0.2);
}

/* Декоративные элементы */
.modal-decor {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  pointer-events: none;
  z-index: 0;
  border-radius: 48px;
}

.decor-leaf {
  position: absolute;
  font-size: 24px;
  opacity: 0.1;
  animation: floatLeaf 8s infinite ease-in-out;
}

.leaf1 { top: 10%; left: 5%; animation-delay: 0s; }
.leaf2 { bottom: 15%; right: 8%; animation-delay: 1.5s; font-size: 30px; }
.leaf3 { top: 30%; right: 12%; animation-delay: 0.8s; font-size: 20px; }

.decor-circle {
  position: absolute;
  border-radius: 50%;
  background: rgba(50, 180, 144, 0.05);
  animation: floatCircle 12s infinite;
}

.circle1 { width: 180px; height: 180px; top: -60px; right: -60px; }
.circle2 { width: 140px; height: 140px; bottom: -40px; left: -40px; animation-delay: 1.5s; }
.circle3 { width: 100px; height: 100px; top: 50%; left: 50%; transform: translate(-50%, -50%); animation-delay: 2s; }

.button-close {
  background: rgba(1, 69, 50, 0.8);
  color: white;
  font-weight: 700;
  font-size: 22px;
  top: 18px;
  right: 18px;
  width: 44px;
  height: 44px;
  border-radius: 50%;
  border: 1px solid rgba(50, 180, 144, 0.3);
  position: absolute;
  cursor: pointer;
  z-index: 10;
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  backdrop-filter: blur(5px);
}

.button-close:hover {
  background: #32b490;
  transform: scale(1.05) rotate(90deg);
  border-color: #55b49a;
}

.modal-icon {
  font-size: 48px;
  margin-bottom: 5px;
  animation: gentleFloat 3s ease-in-out infinite;
  position: relative;
  z-index: 1;
  text-align: center;
}

.section-title {
  margin: 10px 0 14px;
  font-size: 28px;
  font-weight: 900;
  color: #ffffff;
  position: relative;
  z-index: 1;
}

.next-title {
  margin-top: 20px;
}

.current-rank-card {
  background: rgba(1, 69, 48, 0.8);
  backdrop-filter: blur(5px);
  border-radius: 28px;
  padding: 18px 20px;
  border: 1px solid rgba(50, 180, 144, 0.2);
  position: relative;
  z-index: 1;
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
  font-size: 28px;
  font-weight: 900;
  color: white;
  line-height: 1.2;
}

.rank-badge {
  width: 65px;
  height: auto;
  flex-shrink: 0;
}

.xp-block {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.xp-bar {
  width: 100%;
  height: 10px;
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.2);
  overflow: hidden;
}

.xp-fill {
  height: 100%;
  border-radius: 10px;
  background: linear-gradient(90deg, #32b490, #55b49a);
}

.xp-caption {
  margin: 0;
  font-size: 16px;
  font-weight: 400;
  color: rgba(255, 255, 255, 0.8);
  text-align: left;
}

.rank-right {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  flex-shrink: 0;
}

.xp-text {
  margin: 0;
  font-size: 16px;
  font-weight: 400;
  color: white;
}

.next-rank-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
  position: relative;
  z-index: 1;
}

.next-rank-card {
  background: rgba(1, 69, 48, 0.8);
  backdrop-filter: blur(5px);
  border-radius: 24px;
  padding: 14px 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 16px;
  border: 1px solid rgba(50, 180, 144, 0.2);
  transition: all 0.25s ease;
}

.next-rank-card:hover {
  transform: translateX(5px);
  border-color: #32b490;
}

.next-rank-left {
  display: flex;
  flex-direction: column;
  gap: 6px;
  flex: 1;
}

.next-rank-name {
  margin: 0;
  font-size: 20px;
  font-weight: 900;
  color: white;
  line-height: 1.2;
}

.next-rank-range {
  margin: 0;
  font-size: 18px;
  font-weight: 400;
  color: rgba(255, 255, 255, 0.8);
}

.next-rank-caption {
  margin: 0;
  font-size: 15px;
  font-weight: 300;
  color: rgba(255, 255, 255, 0.6);
}

.next-rank-icon {
  width: 55px;
  height: auto;
  flex-shrink: 0;
}

/* ========== АДАПТИВНОСТЬ ========== */
@media (max-width: 768px) {
  .modal {
    max-width: 500px;
    padding: 35px 30px 40px;
  }
  
  .section-title {
    font-size: 26px;
  }
  
  .rank-name {
    font-size: 24px;
  }
  
  .rank-badge {
    width: 55px;
  }
  
  .next-rank-name {
    font-size: 18px;
  }
  
  .next-rank-range {
    font-size: 16px;
  }
  
  .next-rank-caption {
    font-size: 14px;
  }
  
  .next-rank-icon {
    width: 48px;
  }
  
  .button-close {
    width: 38px;
    height: 38px;
    font-size: 20px;
    top: 14px;
    right: 14px;
  }
  
  .modal-icon {
    font-size: 42px;
  }
  
  .decor-leaf {
    font-size: 20px;
    opacity: 0.08;
  }
  
  .leaf2 {
    font-size: 26px;
  }
}

@media (max-width: 576px) {
  .modal-backdrop {
    padding: 15px;
  }
  
  .modal {
    max-width: 95%;
    padding: 30px 20px 35px;
    border-radius: 35px;
  }
  
  .section-title {
    font-size: 24px;
    margin: 5px 0 10px;
  }
  
  .rank-main {
    flex-direction: column;
    align-items: stretch;
  }
  
  .rank-right {
    flex-direction: row;
    justify-content: space-between;
    width: 100%;
  }
  
  .rank-name {
    font-size: 22px;
    margin-bottom: 10px;
  }
  
  .rank-badge {
    width: 50px;
  }
  
  .xp-caption {
    font-size: 14px;
  }
  
  .xp-text {
    font-size: 14px;
  }
  
  .next-rank-card {
    padding: 12px 16px;
  }
  
  .next-rank-name {
    font-size: 16px;
  }
  
  .next-rank-range {
    font-size: 14px;
  }
  
  .next-rank-caption {
    font-size: 12px;
  }
  
  .next-rank-icon {
    width: 42px;
  }
  
  .button-close {
    width: 34px;
    height: 34px;
    font-size: 18px;
    top: 10px;
    right: 10px;
  }
  
  .modal-icon {
    font-size: 36px;
  }
  
  .decor-leaf {
    display: none;
  }
}

@media (max-width: 380px) {
  .modal {
    padding: 25px 15px 30px;
    border-radius: 30px;
  }
  
  .section-title {
    font-size: 22px;
  }
  
  .rank-name {
    font-size: 20px;
  }
  
  .current-rank-card {
    padding: 14px 16px;
  }
  
  .rank-badge {
    width: 45px;
  }
  
  .next-rank-card {
    padding: 10px 14px;
  }
  
  .next-rank-name {
    font-size: 15px;
  }
  
  .next-rank-range {
    font-size: 13px;
  }
  
  .next-rank-icon {
    width: 38px;
  }
  
  .button-close {
    width: 30px;
    height: 30px;
    font-size: 16px;
    top: 8px;
    right: 8px;
  }
  
  .modal-icon {
    font-size: 32px;
  }
}
</style>