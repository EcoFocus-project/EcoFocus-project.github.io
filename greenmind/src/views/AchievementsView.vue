<template>
    <div class="page">
        <div class="header">
          <div class="header-left">
            <img :src="logoImg" alt="логотип" class="logo" />
            <p class="welcome-text">тут хранятся ваши достижения, %nickname%!</p>
          </div>

            <div class="header-buttons">
                <button class="nav-button current-button">
                    <img :src="achievementsImg" alt="достижение" class="nav-icon" />
                    <div class="nav-text">
                        <p>достижения</p>
                    </div>
                </button>

                <button class="nav-button" @click="$emit('go-home')">
                    <img :src="backImg" alt="назад" class="nav-icon" />
                    <div class="nav-text">
                        <p>вернуться назад</p>
                    </div>
                </button>
            </div>
        </div>

        <div class="achievements-content">

            <div class="achievements-card">
                <div class="top-block">
                    <img :src="avatarImg" alt="аватар" class="avatar" />

                    <div class="rank-section">
                        <h2 class="section-title">ваш ранг</h2>

                        <div class="rank-card">
                            <div class="rank-top">
                                <div class="rank-left">
                                    <p class="rank-name">юный эколог</p>
                                    <button class="rank-info-button" @click="$emit('open-rank')">></button>
                                </div>

                                <img :src="badgeImg" alt="бейдж" class="rank-badge">
                            </div>

                            <div class="rank-bottom">
                                <div class="xp-bar">
                                    <div class="xp-fill"></div>
                                </div>
                                <p class="xp-text">0/1000 xp</p>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="divider"></div>

                <div class="slider-section">
                    <button
                    v-if="canSlideLeft"
                    class="slider-arrow left-arrow"
                    @click="slideLeft"
                    >
                        <
                    </button>

                    <div class="cards-viewport">
                        <div 
                            class="cards-track"
                            :style="{transform: `translateX(-${currentStartIndex * (cardWidth + gap)} px)`}"
                        >
                            <div
                                v-for="achievement in achievements" 
                                :key='achievement.id'
                                class="achievement-card"
                                :class="{locked: achievement.locked}"
                            >
                                <h3 class="achievement-title">{{ achievement.title }}</h3>

                                <div class="achievement-icon"></div>

                                <p class="achievement-description">
                                    {{ achievement.description }}
                                </p>
                            </div>
                        </div>
                    </div>

                    <button 
                        v-if='canSlideRight'
                        class="slider-arrow right-arrow"
                        @click="slideRight"
                    >
                        >
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import avatarImg from '../assets/images/avatar.png'
import badgeImg from '../assets/images/rank_icon.png'
import backImg from '../assets/images/back.png'
import achievementsImg from '../assets/images/achievements.png'
import logoImg from '../assets/images/logo.png'
import {computed, ref} from 'vue'
import { gsap } from 'gsap'
import { onMounted } from 'vue';

defineEmits(['go-home', 'open-rank'])

const cardWidth = 300
const gap = 18
const visibleCards = 4

const currentStartIndex = ref(0)

const achievements = [
    {
        id: 1,
        title: 'Друг океанов',
        description: 'пройдите первую локацию',
        locked: false
    },

    {
        id: 2,
        title: 'name_ach',
        description: 'desc_ach',
        locked: true
    },

    {
        id: 3,
        title: 'name_ach',
        description: 'desc_ach',
        locked: true
    },

    {
        id: 4,
        title: 'name_ach',
        description: 'desc_ach',
        locked: true
    },

    {
        id: 5,
        title: 'name_ach',
        description: 'desc_ach',
        locked: true
    },

    {
        id: 6,
        title: 'name_ach',
        description: 'desc_ach',
        locked: true
    }
]

const maxStartIndex = computed(() => {
    return Math.max(0, achievements.length - visibleCards)
})

const canSlideLeft = computed(() => currentStartIndex.value > 0)
const canSlideRight = computed(() => currentStartIndex.value < maxStartIndex.value)

const slideLeft = () => {
    currentStartIndex.value = Math.max(0, currentStartIndex.value - visibleCards)
}

const slideRight = () => {
    currentStartIndex.value = Math.min(
        maxStartIndex.value,
        currentStartIndex.value + visibleCards
    )
}

const animateAchiev = () => {
    const tl = gsap.timeline()

    tl.fromTo(
        '.welcome-text',
        {
            opacity: 0,
            y: -40
        },
        {
            opacity: 1,
            y: 1,
            duration: 0.8,
            ease: 'power4.out'

        }
    )
        .fromTo(
            '.achievements-card',
            {
                opacity: 0,
                y: -60
            },
            {
                opacity: 1,
                y: 0,
                ease: 'power4.out',
                stagger: 0.12
            },
            '-=0.3'
        )
            .fromTo(
            '.slider-section',
            {
                opacity: 0,
                y: -30
            },
            {
                opacity: 1,
                y: 0,
                duration: 0.7,
                ease: 'power4.out'
            },
            '-=0.25'
    )
}

onMounted(() => {
    animateAchiev()
})
</script>

<style scoped>
.page {
  min-height: 100vh;
  background-image: linear-gradient(to bottom, #046A4D, #023522);
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 30px;
  user-select: none;
}

.header-left {
    display: flex;
    flex-direction: row;
    gap: 10px;
    align-items: center;
}

.logo {
    width: 120px;
    height: auto;
    margin: 0 20px;
}

.header-buttons {
  display: flex;
  gap: 20px;
  align-items: center;
}

.nav-button {
  min-width: 260px;
  height: 60px;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  background-color: #0544263f;
  color: white;

  display: flex;
  align-items: center;
  justify-content: flex-start;
  gap: 12px;

  padding: 0 16px;
  text-align: left;

  transition: background-color 0.25s ease, box-shadow 0.25s ease;
}

.nav-button:hover {
  transform: translateY(-4px) scale(1.03);
  background-color: #013b2b;
  box-shadow: 0 12px 24px rgba(0,0,0,0.25);
}

.nav-button:active {
  transform: scale(0.97);
  background-color: #013b2b;
}

.nav-icon {
  width: 50px;
  height: auto;
  flex-shrink: 0;
}

.nav-text {
    display: flex;
    align-items: center;
}

.nav-text p {
    margin: 0;
    font-size: 24px;
    font-weight: 900;
    color: white;
}

.achievements-content {
  padding: 10px 80px 10px;
}

.welcome-text {
  margin: 10px 0 26px;
  font-size: 36px;
  font-weight: 400;
  color: white;
  user-select: none;
}

.achievements-card {
  background-color: #046A4D;
  border-radius: 48px;
  padding: 36px 40px;
  display: flex;
  flex-direction: column;
  gap: 28px;
  min-height: 560px;
  margin: 0 auto;
}

.top-block {
  display: flex;
  align-items: center;
  gap: 32px;
}

.avatar {
    width: 300px;
    height: auto;
}

.rank-section {
  flex: 1;
}

.section-title {
  margin: 0 0 16px;
  font-size: 40px;
  font-weight: 900;
  color: #ffffff;
}

.rank-card {
  background-color: #014530;
  border-radius: 28px;
  padding: 18px 22px;
  display: flex;
  flex-direction: column;
  gap: 18px;
}

.rank-top {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.rank-left {
  display: flex;
  align-items: center;
  gap: 10px;
}

.rank-name {
  margin: 0;
  font-size: 40px;
  font-weight: 900;
  color: white;
}

.rank-info-button {
  background: transparent;
  border: none;
  padding: 0;
  color: white;
  font-size: 24px;
  font-weight: 900;
  cursor: pointer;
  font-family: inherit;
}

.rank-badge {
    width: 70px;
    height: auto;
    flex-shrink: 0;
}

.rank-bottom {
  display: flex;
  align-items: center;
  gap: 16px;
}

.xp-bar {
  flex: 1;
  height: 16px;
  border-radius: 12px;
  background-color: #55b49a;
  overflow: hidden;
}

.xp-text {
  margin: 0;
  font-size: 24px;
  color: white;
  font-weight: 400;
}

.divider {
  width: 100%;
  height: 4px;
  background-color: #013b2b;
  border-radius: 10px;
}

.slider-section {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 18px;
}

.slider-arrow {
  width: 50px;
  height: 50px;
  border: none;
  background: transparent;
  color: #013b2b;
  font-size: 54px;
  font-weight: 900;
  cursor: pointer;
  flex-shrink: 0;
}

.cards-viewport {
  width: 1254px;
  overflow: hidden;
  box-sizing: border-box;
}

.cards-track {
  display: flex;
  gap: 18px;
  transition: transform 0.35s ease;
}

.achievement-card {
  background-color: #013b2b;
  border-radius: 28px;
  padding: 20px 18px 24px;
  min-height: 320px;
  width: 300px;
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.achievement-title {
  margin: 0 0 10px;
  font-size: 40px;
  font-weight: 900;
  color: white;
}

.achievement-subtitle {
  margin: 0 0 18px;
  font-size: 25px;
  font-weight: 400;
  color: white;
  line-height: 1.25;
}

.achievement-icon {
  width: 92px;
  height: 92px;
  border-radius: 50%;
  background-color: #55b49a;
  margin-bottom: 18px;
  flex-shrink: 0;
}

.achievement-description {
  margin: 0;
  font-size: 18px;
  font-weight: 300;
  color: #e6e6e6;
  line-height: 1.35;
}

.inline-link {
  background: transparent;
  border: none;
  padding: 0;
  margin: 0 2px;
  color: #7a7a7a;
  font-size: inherit;
  font-weight: 500;
  font-family: inherit;
  cursor: pointer;
  text-decoration: underline;
}

.inline-link:hover {
  color: #5f5f5f;
}

.current-button {
  background-color: #013b2b;
}

.locked {
  filter: blur(2px);
  opacity: 0.85;
}
</style>