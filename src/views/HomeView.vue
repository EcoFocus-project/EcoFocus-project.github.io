<template>
    <div class="page">
        <div class="header">
            <img :src="logoImg" alt="логотип" class="logo" />
            <div class="header-buttons">
                <button class="nav-button" @click="handleProfileClick">
                    <img :src="profileImg" alt="профиль" class="nav-icon" />
                    <div class="nav-text">
                        <p>мой профиль</p>
                    </div>
                </button>
                <button class="nav-button" @click="handleAchievementsClick">
                    <img :src="achievementsImg" alt="достижение" class="nav-icon" />
                    <div class="nav-text">
                        <p>достижения</p>
                    </div>
                </button>
                <button class="nav-button" @click="handleQuestClick">
                    <img :src="dailyImg" alt="ежедневное задание" class="nav-icon" />
                    <div class="nav-text">
                        <p>ежедневное задание</p>
                    </div>
                </button>
            </div>
        </div>

        <div class="locations">
            <p class="promo-text">выбери, с какой локации начать путешествие!</p>

            <div class="locations-row row-top">
                <button
                v-for="location in topLocations"
                :key="location.id"
                class="location-card"
                :class="{ selected: selectedLocation === location.id }"
                @click="selectLocation(location.id)"
                >
                    <img :src="location.image" :alt="location.title" class="location-icon" />
                    <div class="location-text">
                        <h3>{{ location.title }}</h3>
                        <p>{{ location.description }}</p>
                    </div>
                </button>
            </div>

            <div class="locations-row row-bottom">
                <button 
                v-for="location in bottomLocations"
                :key="location.id" 
                class="location-card"
                :class="{ selected: selectedLocation === location.id }"
                @click="selectLocation(location.id)"
                >
                    <img :src="location.image" :alt="location.title" class="location-icon" />
                    <div class="location-text">
                        <h3>{{ location.title }}</h3>
                        <p>{{ location.description }}</p>
                    </div>
                </button>
            </div>
        </div>

        <div class="footer">
            <button 
            class="start-button"
            :disabled="selectedLocation === null"
            @click="handleStartClick"
            >
                начать
            </button>
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

.nav-icon {
    width: 50px;
    height: auto;
    flex-shrink: 0;
}

.nav-button {
    width: 250px;
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
  background-color: #05442694;
}

.nav-text {
    display: flex;
    align-items: center;
}

.nav-text p {
    margin: 0;
    font-size: 22px;
    line-height: 79%;
    font-weight: 900;
    color: white;
}

.locations {
    margin: 0 50px;
    padding: 0 30px;
}

.promo-text {
    margin-top: 5px;
    font-size: 40px;
    font-weight: 400;
    color: white;
    margin-bottom: 30px;
    user-select: none;
}

.locations-row {
    display: flex;
    justify-content: center;
    gap: 30px;
    position: relative;
    z-index: 1;
    user-select: none;
}

.row-top {
    margin-bottom: 30px;
}

.row-bottom {
    margin-bottom: 40px;
}

.location-card {
    width: 520px;
    height: 245px;
    background-color: #046A4D;
    color: white;
    border: none;
    border-radius: 48px;
    cursor: pointer;
    transition: background-color 0.25s ease, box-shadow 0.25s ease;

    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: flex-start;
    text-align: left;

    padding: 24px 30px 28px;

    position: relative;
    z-index: 1;

}

.location-card:hover {
  transform: translateY(-4px) scale(1.03);
  background-color: #014530;
  box-shadow: 0 12px 24px rgba(0,0,0,0.25);
  z-index: 2;
}

.location-card:active {
  transform: scale(0.97);
  background-color: #023522;
}

.location-card.selected {
    background-color: #023522;
    box-shadow: 0 0 0 4px white, 0 14px 28px rgba(0,0,0,0.28);
    transform: translateY(-4px) scale(1.03);
    z-index: 2;
}

.location-icon {
    width: 100px;
    height: auto;
    flex-shrink: 0;
    margin-bottom: 20px;
}

.location-text {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 10px;
    user-select: none;
}

.location-text h3 {
    margin: 0;
    font-size: 28px;
    font-weight: 900;
    line-height: 73%;
    text-transform: uppercase;
}

.location-text p {
    margin: 0;
    font-size: 20px;
    font-weight: 400;
    line-height: auto;
}

.footer {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin-top: 70px;
}

.start-button {
    width: 340px;
    height: 80px;
    font-size: 48px;
    border: none;
    border-radius: 20px;
    cursor: pointer;
    font-weight: 900;
    background-color: #022e1e;
    color: white;
    transition: background-color 0.25s ease, box-shadow 0.25s ease;
}

.start-button:hover {
  transform: translateY(-4px) scale(1.03);
  background-color: #022417;
  box-shadow: 0 12px 24px rgba(0,0,0,0.25);
}

.start-button:active {
  transform: scale(0.97);
  background-color: #022e1e;
}

</style>

<script setup>
import cityImg from '../assets/images/city.png'
import gardenImg from '../assets/images/garden.png'
import iceImg from '../assets/images/ice.png'
import forestImg from '../assets/images/forest.png'
import oceanImg from '../assets/images/ocean.png'
import dailyImg from '../assets/images/daily.png'
import achievementsImg from '../assets/images/achievements.png'
import profileImg from '../assets/images/profile.png'
import logoImg from '../assets/images/logo.png'
import {computed, ref} from 'vue'
import {gsap} from 'gsap'
import { onMounted } from 'vue'

const props = defineProps ({
    userMode: {
        type: String,
        required: true
    }
})

const emit = defineEmits([
    'open-restricted',
    'open-in-development',
    'open-first-location',
    'open-profile',
    'open-achievements'
])

const selectedLocation = ref (null)

const locations = [
    {
        id: 1,
        title: 'Друг океанов',
        description: 'Погрузись в подводный мир и помоги морским обитателям сохранить чистоту океана.',
        image: oceanImg
    },
    {
        id: 2,
        title: 'Хранитель леса',
        description: 'Исследуй зелёный лес и узнай, как защищать деревья, животных и лесные тропы.',
        image: forestImg
    },
    {
        id: 3,
        title: 'Ледяной край',
        description: 'Отправляйся в мир снега и льда и узнай, почему так важно сохранять холодные экосистемы.',
        image: iceImg
    },
    {
        id: 4,
        title: 'Сад жизни',
        description: 'Выращивай растения, наблюдай за природой и учись заботиться о мире вокруг себя.',
        image: gardenImg
    },
    {
        id: 5,
        title: 'Город будущего',
        description: 'Узнай, как экология, транспорт и технологии могут сделать город чище и удобнее для всех.',
        image: cityImg
    }
]

const topLocations = computed(() => locations.slice(0, 3))
const bottomLocations = computed(() => locations.slice(3))

const selectLocation = (id) => {
    selectedLocation.value = id
}

const handleProfileClick = () => {
    if (props.userMode === 'authorized') {
        emit('open-profile')
    } else {
        emit('open-restricted')
    }
}

const handleAchievementsClick = () => {
    if (props.userMode === 'authorized') {
        emit('open-achievements')
    } else {
        emit('open-restricted')
    }
}
const handleQuestClick = () => {
    emit('open-in-development')
}

const handleStartClick = () => {
    if (selectedLocation.value === 1) {
        emit('open-first-location')
    } else if (selectedLocation.value !== null) {
        emit('open-in-development')
    }
}


const animateHome = () => {
    const tl = gsap.timeline()

    tl.fromTo(
        '.promo-text',
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
            '.location-card',
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
            '.start-button',
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
    animateHome()
})
</script>