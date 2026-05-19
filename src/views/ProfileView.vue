<template>
    <div class="page">
        <div class="header">
            <div class="header-left">
                <img :src="logoImg" alt="логотип" class="logo" />
                <p class="welcome-text">с возвращением, %nickname%!</p>
            </div>

            <div class="header-buttons">
                <button class="nav-button current-button">
                    <div class="nav-text">
                        <img :src="profileImg" alt="профиль" class="nav-icon" />
                        <p>мой профиль</p>
                    </div>
                </button>

                <button class="nav-button">
                    <div class="nav-text" @click="$emit('go-home')">
                        <img :src="backImg" alt="назад" class="nav-icon" />
                        <p>вернуться назад</p>
                    </div>
                </button>
            </div>
        </div>

        <div class="profile-content">

            <div class="profile-card">
                <div class="profile-left">
                    <div class="avatar-block">
                        <img :src="avatarImg" alt="аватар" class="avatar" />
                    </div>

                    <div class="profile-actions">
                        <button class="action-button" @click="$emit('open-edit-profile')">редактировать профиль</button>
                        <button class="action-button" @click="$emit('open-account-settings')">настройки аккаунта</button>
                        <button class="action-button" @click="$emit('open-logout')">выйти <br> из аккаунта</button>
                    </div>
                </div>

                <div class="profile-divider"></div>

                <div class="profile-right">
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

                    <div class="stats-section">
                        <h2 class="section-title">статистика</h2>

                        <div class="stats-grid">
                            <div class="stat-card">
                                <img :src="statDayImg" alt="1" class="stat-icon" />
                                <h3 class="stat-title">1 день</h3>
                                <p class="stat-subtitle">вы заходите подряд.</p>
                                <p class="stat-link-text">ваша серия за все время -<br> 1 день</p>
                            </div>

                            <div class="stat-card">
                                <img :src="statLocImg" alt="1" class="stat-icon" />
                                <h3 class="stat-title">n/5</h3>
                                <p class="stat-subtitle">пройденных локаций!</p>
                                <p class="stat-link-text">
                                    продолжим путешествие 
                                    <button class="inline-link" @click="$emit('go-home')">вместе?</button>
                                </p>
                            </div>
                            <div class="stat-card">
                                <img :src="statAchImg" alt="1" class="stat-icon" />
                                <h3 class="stat-title">друг океанов</h3>
                                <p class="stat-subtitle">недавняя награда</p>
                                <p class="stat-link-text">
                                    вы можете изучить другие
                                    <button class="inline-link" @click="$emit('open-achievements')">достижения</button>
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

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
    padding-right: 10px;
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

.profile-content {
    padding: 10px 70px 10px;
}

.welcome-text {
    font-size: 36px;
    font-weight: 400;
    color: white;
    user-select: none;
}

.profile-card {
    background-color: #046A4D;
    border-radius: 48px;
    padding: 36px 40px;
    display: flex;
    gap: 28px;
    min-height: 560px;
    margin: 0 auto;
}

.profile-left {
    width: 400px;
    display: flex;
    flex-direction: column;
    align-items: center;
}

.avatar-block {
    position: relative;
    margin-bottom: 40px;
}

.avatar {
    width: 300px;
    height: auto;
}

.profile-actions {
    width: 100%;
    display: flex;
    flex-direction: column;
    gap: 34px;
    align-items: center;
}

.action-button {
    width: 270px;
    min-height: 60px;
    border: none;
    border-radius: 20px;
    background-color: #013b2b;
    color: white;
    font-size: 24px;
    font-weight: 900;
    cursor: pointer;
    padding: 0 16px;
    line-height: 1.1;
}

.action-button:hover {
    background-color: #012216;
    box-shadow: 0 12px 24px rgba(0,0,0,0.25);
}

.profile-divider {
    width: 4px;
    background-color: #013b2b;
    border-radius: 10px;
}

.profile-right {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 32px;
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

.stats-grid {
    display: grid;
    grid-template-columns: repeat(3, 320px);
    gap: 18px;
}

.stat-card {
    background-color: #014530;
    border-radius: 28px;
    padding: 20px 18px 24px;
    min-height: 320px;
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    max-width: 300px;
}

.stat-title {
    margin: 0 0 10px;
    font-size: 40px;
    font-weight: 900;
    color: white;
}

.stat-subtitle {
    margin: 0 0 18px;
    font-size: 25px;
    font-weight: 400;
    color: white;
}

.stat-icon {
    width: 140px;
    height: auto;
    margin-bottom: 18px;
}

.stat-link-text {
    margin: 0;
    font-size: 18px;
    font-weight: 300;
    color: #fff;
}

.inline-link {
    background: transparent;
    border: none;
    padding: 0;
    margin: 0 2px;
    color: #c1d1ce;
    font-size: inherit;
    font-weight: 500;
    cursor: pointer;
    text-decoration: underline;
}

.inline-link:hover {
    color: #D1D0D1;
}

.current-button {
    background-color:  #013b2b;
}

</style>

<script setup>
import badgeImg from '../assets/images/rank_icon.png'
import statAchImg from '../assets/images/stat_3.png'
import statLocImg from '../assets/images/stat_2.png'
import statDayImg from '../assets/images/stat_1.png'
import avatarImg from '../assets/images/avatar.png'
import backImg from '../assets/images/back.png'
import profileImg from '../assets/images/profile.png'
import logoImg from '../assets/images/logo.png'
import { gsap } from 'gsap'
import { ref , onMounted } from 'vue';
defineEmits(['go-home', 'open-achievements', 'open-logout', 'open-rank', 'open-account-settings', 'open-edit-profile'])

const animateProfile = () => {
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
            '.rank-card',
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
            '.stats-section',
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
    animateProfile()
})
</script>