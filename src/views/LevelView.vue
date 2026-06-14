<template>
  <div class="ocean-level-container">
    <div class="waves">
      <div class="wave wave1"></div>
      <div class="wave wave2"></div>
      <div class="wave wave3"></div>
    </div>

    <button class="back-button" @click="$emit('go-home')">← Назад</button>

    <div class="level-header">
      <div class="level-badge">
        <span class="level-icon">🌊</span>
        <span class="level-title">Уровень 2: Сортировка океана</span>
      </div>
      <div class="score-card">
        <span class="score-icon">⭐</span>
        <span class="score-text">{{ collectedCount }} / {{ totalTargets }}</span>
        <span class="score-label">предметов спасено</span>
      </div>
    </div>

    <div class="game-layout">
      <div class="drop-zone aquarium" @dragover.prevent @drop="handleDrop($event, 'aquarium')">
        <div class="zone-title">
          <span class="zone-icon">🐠</span>
          <h3>Аквариум (живые)</h3>
        </div>
        <div class="zone-items">
          <div v-for="item in aquariumItems" :key="item.id" class="zone-item">
            <span class="item-emoji">{{ item.emoji }}</span>
            <span class="item-name">{{ item.name }}</span>
          </div>
        </div>
      </div>

      <div class="drop-zone trash-bin" @dragover.prevent @drop="handleDrop($event, 'trash')">
        <div class="zone-title">
          <span class="zone-icon">🗑️</span>
          <h3>Корзина (мусор)</h3>
        </div>
        <div class="zone-items">
          <div v-for="item in trashItems" :key="item.id" class="zone-item">
            <span class="item-emoji">{{ item.emoji }}</span>
            <span class="item-name">{{ item.name }}</span>
          </div>
        </div>
      </div>
    </div>

    <div class="floating-items">
      <h3 class="section-title">🌊 Плавающие объекты (нажми и удерживай)</h3>
      <div class="drag-items-container">
        <div
          v-for="item in dragItems"
          :key="item.id"
          class="drag-item"
          :class="item.type === 'trash' ? 'item-trash' : 'item-living'"
          draggable="true"
          @dragstart="handleDragStart($event, item)"
          @dragend="handleDragEnd"
          @touchstart="handleTouchStart($event, item)"
          @touchmove="handleTouchMove"
          @touchend="handleTouchEnd"
        >
          <span class="drag-emoji">{{ item.emoji }}</span>
          <span class="drag-name">{{ item.name }}</span>
        </div>
      </div>
    </div>

    <div v-if="isComplete" class="completion-modal">
      <div class="completion-content">
        <span class="completion-icon">🐬✨</span>
        <h2>Уровень пройден!</h2>
        <p>Ты правильно рассортировал всех обитателей и мусор!</p>
        <p class="reward-text">+250 XP</p>
        <div class="completion-buttons">
          <button class="next-level-btn" @click="goToNextLevel">Следующий уровень →</button>
          <button class="home-btn" @click="$emit('go-home')">Вернуться домой</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LevelTwoView',
  props: {
    userMode: {
      type: String,
      required: true
    },
    levelId: {
      type: Number,
      default: 2
    }
  },
  data() {
    return {
      currentDragItem: null,
      draggedElement: null,
      touchTarget: null,
      
      items: [
        { id: 1, name: 'Медуза аурелия', emoji: '🪼', type: 'living', zone: null },
        { id: 2, name: 'Медуза цианея', emoji: '🪼', type: 'living', zone: null },
        { id: 3, name: 'Рыбка-клоун', emoji: '🐠', type: 'living', zone: null },
        { id: 4, name: 'Морской конёк', emoji: '🐴', type: 'living', zone: null },
        { id: 5, name: 'Черепаха', emoji: '🐢', type: 'living', zone: null },
        { id: 6, name: 'Пластиковый пакет', emoji: '🛍️', type: 'trash', zone: null },
        { id: 7, name: 'Пластиковая бутылка', emoji: '🧴', type: 'trash', zone: null },
        { id: 8, name: 'Рыболовная сеть', emoji: '🎣', type: 'trash', zone: null },
        { id: 9, name: 'Алюминиевая банка', emoji: '🥫', type: 'trash', zone: null }
      ],
      
      correctPlacements: {
        living: 'aquarium',
        trash: 'trash'
      },
      
      showCompleteModal: false
    }
  },
  
  computed: {
    dragItems() {
      return this.items.filter(item => item.zone === null)
    },
    
    aquariumItems() {
      return this.items.filter(item => item.zone === 'aquarium')
    },
    
    trashItems() {
      return this.items.filter(item => item.zone === 'trash')
    },
    
    collectedCount() {
      return this.items.filter(item => item.zone === 'aquarium' || item.zone === 'trash').length
    },
    
    totalTargets() {
      return this.items.length
    },
    
    isComplete() {
      return this.dragItems.length === 0 && !this.showCompleteModal
    }
  },
  
  watch: {
    isComplete(val) {
      if (val) {
        this.completeLevel()
      }
    }
  },
  
  methods: {
    // Десктопная версия
    handleDragStart(event, item) {
      this.currentDragItem = item
      event.dataTransfer.setData('text/plain', JSON.stringify(item))
      event.dataTransfer.effectAllowed = 'move'
      event.target.classList.add('dragging')
    },
    
    handleDragEnd(event) {
      event.target.classList.remove('dragging')
      this.currentDragItem = null
    },
    
    handleDrop(event, targetZone) {
      event.preventDefault()
      
      if (!this.currentDragItem) return
      
      this.processDrop(this.currentDragItem, targetZone)
      this.currentDragItem = null
    },
    
    // Мобильная версия (touch)
    handleTouchStart(event, item) {
      event.preventDefault()
      this.currentDragItem = item
      this.touchTarget = event.target.closest('.drag-item')
      if (this.touchTarget) {
        this.touchTarget.classList.add('dragging')
      }
      
      // Создаём клон для визуального отображения
      const rect = this.touchTarget.getBoundingClientRect()
      const touch = event.touches[0]
      
      // Запоминаем позицию для перемещения
      this.touchStartX = touch.clientX - rect.left
      this.touchStartY = touch.clientY - rect.top
    },
    
    handleTouchMove(event) {
      if (!this.currentDragItem || !this.touchTarget) return
      event.preventDefault()
      
      const touch = event.touches[0]
      this.touchTarget.style.position = 'fixed'
      this.touchTarget.style.zIndex = '9999'
      this.touchTarget.style.left = (touch.clientX - this.touchStartX) + 'px'
      this.touchTarget.style.top = (touch.clientY - this.touchStartY) + 'px'
      this.touchTarget.style.opacity = '0.8'
    },
    
    handleTouchEnd(event) {
      if (!this.currentDragItem) return
      event.preventDefault()
      
      // Определяем, над какой зоной произошёл отпуск
      const touch = event.changedTouches[0]
      const elementUnderTouch = document.elementFromPoint(touch.clientX, touch.clientY)
      const dropZone = elementUnderTouch?.closest('.drop-zone')
      
      let targetZone = null
      if (dropZone) {
        if (dropZone.classList.contains('aquarium')) {
          targetZone = 'aquarium'
        } else if (dropZone.classList.contains('trash-bin')) {
          targetZone = 'trash'
        }
      }
      
      if (targetZone) {
        this.processDrop(this.currentDragItem, targetZone)
      } else {
        this.showFeedback(false)
      }
      
      // Возвращаем элемент в исходное состояние
      if (this.touchTarget) {
        this.touchTarget.style.position = ''
        this.touchTarget.style.zIndex = ''
        this.touchTarget.style.left = ''
        this.touchTarget.style.top = ''
        this.touchTarget.style.opacity = ''
        this.touchTarget.classList.remove('dragging')
      }
      
      this.currentDragItem = null
      this.touchTarget = null
    },
    
    processDrop(item, targetZone) {
      const expectedZone = this.correctPlacements[item.type]
      
      if (targetZone === expectedZone) {
        const index = this.items.findIndex(i => i.id === item.id)
        if (index !== -1) {
          this.items[index].zone = targetZone
        }
        this.showFeedback(true, targetZone)
      } else {
        this.showFeedback(false)
      }
    },
    
    showFeedback(isCorrect, zone = null) {
      if (isCorrect) {
        const dropZone = document.querySelector(zone === 'aquarium' ? '.aquarium' : '.trash-bin')
        if (dropZone) {
          dropZone.classList.add('correct-flash')
          setTimeout(() => dropZone.classList.remove('correct-flash'), 500)
        }
      } else {
        const dragItems = document.querySelector('.drag-items-container')
        dragItems.classList.add('wrong-shake')
        setTimeout(() => dragItems.classList.remove('wrong-shake'), 500)
      }
    },
    
    completeLevel() {
      this.showCompleteModal = true
      
      const completedLevels = JSON.parse(localStorage.getItem('completedLevels') || '[]')
      if (!completedLevels.includes(2)) {
        completedLevels.push(2)
        localStorage.setItem('completedLevels', JSON.stringify(completedLevels))
      }
      
      const userXP = parseInt(localStorage.getItem('userXP') || '0')
      localStorage.setItem('userXP', userXP + 250)
    },
    
    goToNextLevel() {
      this.$emit('complete')
      this.$emit('go-home')
    }
  }
}
</script>

<style scoped>
@keyframes wave {
  0% { transform: translateX(0) translateZ(0) scaleY(1); }
  50% { transform: translateX(-25%) translateZ(0) scaleY(0.8); }
  100% { transform: translateX(-50%) translateZ(0) scaleY(1); }
}

@keyframes correctFlash {
  0%, 100% { border-color: rgba(50, 180, 144, 0.3); background: rgba(50, 180, 144, 0.1); }
  50% { border-color: #4caf50; background: rgba(76, 175, 80, 0.3); box-shadow: 0 0 20px #4caf50; }
}

@keyframes wrongShake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-5px); }
  75% { transform: translateX(5px); }
}

@keyframes gentleFloat {
  0%, 100% { transform: translateY(0px); }
  50% { transform: translateY(-6px); }
}

.ocean-level-container {
  min-height: 100vh;
  background: linear-gradient(135deg, #0c376a 0%, #042145 50%, #01152b 100%);
  padding: 80px 20px 20px 20px;
  position: relative;
  overflow-x: hidden;
}

.waves {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 150px;
  pointer-events: none;
  z-index: 0;
}

.wave {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 200%;
  height: 100%;
  background-repeat: repeat-x;
  background-size: 50% 100%;
  opacity: 0.3;
}

.wave1 {
  background: linear-gradient(transparent 60%, rgba(64, 224, 208, 0.4) 100%);
  animation: wave 8s cubic-bezier(0.36, 0.45, 0.63, 0.53) infinite;
}

.wave2 {
  background: linear-gradient(transparent 70%, rgba(72, 209, 204, 0.3) 100%);
  animation: wave 12s cubic-bezier(0.36, 0.45, 0.63, 0.53) -3s infinite;
}

.wave3 {
  background: linear-gradient(transparent 80%, rgba(32, 178, 170, 0.2) 100%);
  animation: wave 16s cubic-bezier(0.36, 0.45, 0.63, 0.53) -6s infinite;
}

.back-button {
  position: fixed;
  top: 20px;
  left: 20px;
  padding: 10px 20px;
  background: rgba(27, 51, 79, 0.9);
  backdrop-filter: blur(10px);
  color: white;
  border: 1px solid rgba(64, 224, 208, 0.5);
  border-radius: 30px;
  cursor: pointer;
  font-size: 16px;
  font-weight: bold;
  z-index: 10;
  transition: all 0.3s;
}

.back-button:hover {
  background: #042145;
  transform: translateX(-5px);
}

.level-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 30px;
  flex-wrap: wrap;
  gap: 15px;
  position: relative;
  z-index: 2;
}

.level-badge {
  background: rgba(27, 51, 79, 0.8);
  backdrop-filter: blur(10px);
  padding: 12px 20px;
  border-radius: 40px;
  border: 1px solid rgba(64, 224, 208, 0.3);
}

.level-icon {
  font-size: 24px;
  margin-right: 10px;
}

.level-title {
  font-size: 18px;
  font-weight: bold;
  color: white;
}

.score-card {
  background: rgba(27, 51, 79, 0.8);
  backdrop-filter: blur(10px);
  padding: 12px 25px;
  border-radius: 40px;
  border: 1px solid rgba(64, 224, 208, 0.3);
  display: flex;
  align-items: center;
  gap: 10px;
}

.score-icon {
  font-size: 24px;
}

.score-text {
  font-size: 24px;
  font-weight: bold;
  color: #ffd700;
}

.score-label {
  color: white;
  font-size: 14px;
}

.game-layout {
  display: flex;
  gap: 30px;
  margin-bottom: 30px;
  position: relative;
  z-index: 2;
  flex-wrap: wrap;
}

.drop-zone {
  flex: 1;
  min-width: 280px;
  background: rgba(4, 33, 69, 0.6);
  backdrop-filter: blur(10px);
  border-radius: 30px;
  padding: 20px;
  border: 2px dashed rgba(64, 224, 208, 0.5);
  transition: all 0.3s;
}

.drop-zone:hover {
  border-color: #40e0d0;
  background: rgba(4, 33, 69, 0.8);
}

.correct-flash {
  animation: correctFlash 0.5s ease;
}

.zone-title {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 15px;
  padding-bottom: 10px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
}

.zone-title h3 {
  color: white;
  font-size: 20px;
  margin: 0;
}

.zone-icon {
  font-size: 28px;
}

.zone-items {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  min-height: 100px;
}

.zone-item {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 15px;
  padding: 8px 12px;
  display: flex;
  align-items: center;
  gap: 8px;
  border: 1px solid rgba(64, 224, 208, 0.3);
}

.item-emoji {
  font-size: 24px;
}

.item-name {
  color: white;
  font-size: 12px;
}

.floating-items {
  background: rgba(27, 51, 79, 0.8);
  backdrop-filter: blur(10px);
  border-radius: 30px;
  padding: 20px;
  position: relative;
  z-index: 2;
}

.section-title {
  color: white;
  font-size: 20px;
  margin-bottom: 20px;
  text-align: center;
}

.drag-items-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 15px;
}

.drag-item {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 20px;
  padding: 12px 20px;
  display: flex;
  align-items: center;
  gap: 12px;
  cursor: grab;
  transition: all 0.3s;
  border: 2px solid rgba(64, 224, 208, 0.3);
  user-select: none;
  -webkit-tap-highlight-color: transparent;
}

.drag-item:active {
  cursor: grabbing;
}

.drag-item:hover {
  transform: translateY(-5px);
  background: rgba(255, 255, 255, 0.2);
  border-color: #40e0d0;
}

.drag-item.dragging {
  opacity: 0.5;
  cursor: grabbing;
}

.drag-emoji {
  font-size: 36px;
}

.drag-name {
  color: white;
  font-size: 16px;
  font-weight: 500;
}

.item-trash {
  border-color: rgba(244, 67, 54, 0.3);
}

.item-trash:hover {
  border-color: #f44336;
}

.item-living {
  border-color: rgba(76, 175, 80, 0.3);
}

.item-living:hover {
  border-color: #4caf50;
}

.wrong-shake {
  animation: wrongShake 0.5s ease;
}

.completion-modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.85);
  backdrop-filter: blur(10px);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  animation: fadeIn 0.5s;
}

.completion-content {
  background: linear-gradient(135deg, #1b5e8c, #0c376a);
  padding: 40px;
  border-radius: 30px;
  text-align: center;
  max-width: 500px;
  border: 2px solid #40e0d0;
  animation: slideUp 0.5s;
}

.completion-icon {
  font-size: 64px;
  animation: gentleFloat 2s infinite;
}

.completion-content h2 {
  color: #ffd700;
  font-size: 32px;
  margin: 20px 0;
}

.completion-content p {
  color: white;
  font-size: 18px;
  margin: 10px 0;
}

.reward-text {
  font-size: 28px !important;
  color: #ffd700 !important;
  font-weight: bold;
  margin: 20px 0 !important;
}

.completion-buttons {
  display: flex;
  gap: 15px;
  justify-content: center;
  margin-top: 20px;
  flex-wrap: wrap;
}

.next-level-btn {
  background: linear-gradient(135deg, #40e0d0, #008080);
  color: white;
  border: none;
  padding: 12px 25px;
  border-radius: 40px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s;
}

.next-level-btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
}

.home-btn {
  background: rgba(27, 51, 79, 0.8);
  color: white;
  border: 1px solid #40e0d0;
  padding: 12px 25px;
  border-radius: 40px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s;
}

.home-btn:hover {
  background: #042145;
  transform: translateY(-3px);
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideUp {
  from { transform: translateY(50px); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}

@media (max-width: 768px) {
  .ocean-level-container {
    padding: 70px 15px 15px 15px;
  }
  
  .game-layout {
    flex-direction: column;
  }
  
  .drop-zone {
    min-width: auto;
  }
  
  .drag-name {
    font-size: 14px;
  }
  
  .drag-emoji {
    font-size: 28px;
  }
  
  .score-text {
    font-size: 18px;
  }
  
  .level-title {
    font-size: 14px;
  }
  
  .drag-item {
    padding: 10px 16px;
  }
  
  .drag-item:active {
    cursor: grabbing;
  }
}
</style>