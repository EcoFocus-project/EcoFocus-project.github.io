<template>
  <WelcomeView 
    v-if="currentScreen === 'welcome'"
    @open-login="openAuthModal('login')" 
    @continue-guest="enterGuestMode"
  />
  <HomeView
    v-if="currentScreen === 'home'"
    :user-mode="userMode"
    @open-restricted="openRestrictedModal"
    @open-in-development="openInDevelopmentModal"
    @open-first-location="openFirstLocation"
    @open-profile="openProfile"
    @open-achievements="openAchievements"
  />
  <ProfileView 
    v-if="currentScreen === 'profile'"
    @go-home="goHome"
    @open-achievements="openAchievements"
    @open-logout="openLogOutModal"
    @open-rank="openRankModal"
    @open-account-settings="openAccountSettings"
    @open-edit-profile="openEditProfile"
  />

  <AuthModal
    v-if = 'isAuthOpen'
    :mode= 'authMode'
    @close="closeAuthModal"
    @switch-mode="switchAuthMode"
    @success-login="enterAuthorizedMode"
  />

  <RestrictedModal 
    v-if ='isRestrictedOpen'
    @close="closeRestrictedModal"
    @open-login="openLoginFromRestricted"
    @open-register="openRegisterFromRestricted"
  />

  <InDevelopmentModal 
    v-if="isInDevelopmentOpen"
    @close="closeInDevelopmentModal"
  />

  <AchievementsView
    v-if="currentScreen === 'achievements'"
    @go-home="goHome"
    @open-rank="openRankModal"
  />

  <OceanIntroView
    v-if="currentScreen === 'ocean-intro'"
    @go-home="goHome"
    @open-level-one="openLevelOne"
    :user-mode="userMode"
    @open-restricted="openRestrictedModal"
    @open-in-development="openInDevelopmentModal"
    @open-first-location="openFirstLocation"
    @open-profile="openProfile"
    @open-achievements="openAchievements"
  />    

  <LogOutModal 
    v-if="isLogOutOpen"
    @close="closeLogOutModal"
    @confirm-logout="confirmLogOut"
  
  />

  <RankModal
    v-if="isRankOpen"
    @close="closeRankModal"
  />

  <AccountSettingsView 
    v-if="currentScreen === 'account-settings'"
    @open-profile="openProfile"
    @open-edit-profile="openEditProfile"
    @open-logout="openLogOutModal"
    @open-deactivate="openDeactivateModal"
    @go-home="goHome"
  />

  <EditProfileView 
    v-if="currentScreen === 'edit-profile'"
    @open-profile="openProfile"
    @go-home="goHome"
  />

  <DeactiveModal 
    v-if='isDeactivateOpen'
    @close="closeDeactivateModal"
    @confirm-deactivate="confirmDeactivate"
  />

  <LevelView
  v-if="currentScreen === 'level-one'"
  @complete="onLevelComplete"
  @go-home="goHome"
  />

</template>

<script setup>
import { ref } from 'vue'
import WelcomeView from './views/WelcomeView.vue'
import AuthModal from './components/AuthModal.vue'
import HomeView from './views/HomeView.vue'
import RestrictedModal from './components/RestrictedModal.vue'
import InDevelopmentModal from './components/InDevelopmentModal.vue'
import ProfileView from './views/ProfileView.vue'
import AchievementsView from './views/AchievementsView.vue'
import OceanIntroView from './views/OceanIntroView.vue'
import LogOutModal from './components/LogOutModal.vue'
import RankModal from './components/RankModal.vue'
import AccountSettingsView from './views/AccountSettingsView.vue'
import EditProfileView from './views/EditProfileView.vue'
import DeactiveModal from './components/DeactiveModal.vue'
import LevelView from './views/LevelView.vue'


const isRestrictedOpen = ref(false)
const isInDevelopmentOpen = ref(false)

const currentScreen = ref('welcome') //переменная для хранения текущего экрана приложения
const userMode = ref('guest') //переменная, определяющая режим пользователя (guest - гость, authorized - авторизованный)
const isAuthOpen = ref(false) // переменная, отвечающая за открытие и закрытие модального окна авторизации
const authMode = ref('login') //переменная для хранения текущего режима авторизации(login - вход /register - регистрация)

const openAuthModal = (mode) => {
  authMode.value = mode
  isAuthOpen.value = true
}

const closeAuthModal = () => {
  isAuthOpen.value = false
}

const switchAuthMode = (mode) => {
  authMode.value = mode
}



const enterGuestMode = () => {
  userMode.value = 'guest' // переход в гостевой режим
  currentScreen.value = 'home' // пользователь получает ограниченный доступ и перенаправляется на главный экран
}

const enterAuthorizedMode = () => {
  userMode.value = 'authorized' // Переход в режим авторизованного пользователя
  isAuthOpen.value = false // После успешного входа модальное окно закрывается,
  currentScreen.value = 'home' // пользователь перенаправляется на главный экран
}

const openRestrictedModal = () => {
  isRestrictedOpen.value = true
}

const closeRestrictedModal = () => {
  isRestrictedOpen.value = false
}

const openInDevelopmentModal = () => {
  isInDevelopmentOpen.value = true
}

const closeInDevelopmentModal = () => {
  isInDevelopmentOpen.value = false
}

const openAuthFromRestricted = () => {
  isRestrictedOpen.value = false
  openAuthModal('login')
}

const openLoginFromRestricted = () => {
  isRestrictedOpen.value = false
  openAuthModal('login')
}

const openRegisterFromRestricted = () => {
  isRestrictedOpen.value = false
  openAuthModal('register')
}

const openFirstLocation = () => {
  currentScreen.value = 'ocean-intro'
}

const openLevelOne = () => {
  currentScreen.value = 'level-one'
}

const openProfile = () => {
  if (userMode.value === 'authorized') {
    currentScreen.value = 'profile'
  } else {
    openRestrictedModal()
  }
}

const goHome = () => {
  currentScreen.value = 'home'
}

const openAchievements = () => {
  currentScreen.value = 'achievements'
}

const isLogOutOpen = ref(false)

const openLogOutModal = () => {
  isLogOutOpen.value = true
}

const closeLogOutModal = () => {
  isLogOutOpen.value = false
}

const confirmLogOut = () => {
  isLogOutOpen.value = false
  userMode.value = 'guest'
  currentScreen.value = 'welcome'
}

const isRankOpen = ref(false)

const openRankModal = () => {
  isRankOpen.value = true
}

const closeRankModal = () => {
  isRankOpen.value = false
}

const openAccountSettings = () => {
  currentScreen.value = 'account-settings'
}

const openEditProfile = () => {
  currentScreen.value = 'edit-profile'
}

const isDeactivateOpen = ref(false)

const openDeactivateModal = () => {
  isDeactivateOpen.value = true
}

const closeDeactivateModal = () => {
  isDeactivateOpen.value = false
}

const confirmDeactivate = () => {
  isDeactivateOpen.value = false
  userMode.value = 'guest'
  currentScreen.value = 'welcome'
}

</script>