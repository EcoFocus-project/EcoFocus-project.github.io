<template>
  <div class="ocean-level-container">
    <div class="waves">
      <div class="wave wave1"></div>
      <div class="wave wave2"></div>
      <div class="wave wave3"></div>
    </div>

    <button class="back-button" @click="goHome">← Назад</button>

    <div class="level-header">
      <div class="level-badge">
        <span class="level-icon">🔗</span>
        <span class="level-title">Уровень 2: Пищевая цепочка</span>
      </div>
      <div class="score-card">
        <span class="score-icon">⭐</span>
        <span class="score-text">{{ correctChains }} / {{ totalChains }}</span>
        <span class="score-label">цепочек собрано</span>
      </div>
      <div class="mistake-card" v-if="mistakes > 0">
        <span class="mistake-icon">❌</span>
        <span class="mistake-text">{{ mistakes }}</span>
        <span class="mistake-label">ошибок</span>
      </div>
    </div>

    <div class="game-layout">
      <!-- Колонка 1: ХИЩНИКИ -->
      <div class="chain-column">
        <div class="column-header">
          <span class="column-icon">🦈</span>
          <h3>ХИЩНИКИ</h3>
        </div>
        <div class="column-content">
          <div 
            v-for="slot in predatorSlots" 
            :key="slot.id"
            class="chain-slot"
            :class="{ filled: slot.filled, correct: slot.correct, wrong: slot.wrong }"
            @click="isMobile ? selectSlot(slot, 'predator') : null"
            @dragover.prevent
            @drop="handleDrop($event, slot, 'predator')"
          >
            <div v-if="slot.item" class="slot-item">
              <span class="slot-emoji">{{ slot.item.emoji }}</span>
              <span class="slot-name">{{ slot.item.name }}</span>
            </div>
            <div v-else class="empty-slot">⬅️ перетащи сюда</div>
          </div>
        </div>
      </div>

      <div class="chain-arrow">→</div>

      <!-- Колонка 2: ЖЕРТВЫ -->
      <div class="chain-column">
        <div class="column-header">
          <span class="column-icon">🐟</span>
          <h3>ЖЕРТВЫ</h3>
        </div>
        <div class="column-content">
          <div 
            v-for="slot in preySlots" 
            :key="slot.id"
            class="chain-slot"
            :class="{ filled: slot.filled, correct: slot.correct, wrong: slot.wrong }"
            @click="isMobile ? selectSlot(slot, 'prey') : null"
            @dragover.prevent
            @drop="handleDrop($event, slot, 'prey')"
          >
            <div v-if="slot.item" class="slot-item">
              <span class="slot-emoji">{{ slot.item.emoji }}</span>
              <span class="slot-name">{{ slot.item.name }}</span>
            </div>
            <div v-else class="empty-slot">⬅️ перетащи сюда</div>
          </div>
        </div>
      </div>

      <div class="chain-arrow">→</div>

      <!-- Колонка 3: ОТХОДЫ -->
      <div class="chain-column">
        <div class="column-header">
          <span class="column-icon">♻️</span>
          <h3>ОТХОДЫ</h3>
        </div>
        <div class="column-content">
          <div 
            v-for="slot in wasteSlots" 
            :key="slot.id"
            class="chain-slot"
            :class="{ filled: slot.filled, correct: slot.correct, wrong: slot.wrong }"
            @click="isMobile ? selectSlot(slot, 'waste') : null"
            @dragover.prevent
            @drop="handleDrop($event, slot, 'waste')"
          >
            <div v-if="slot.item" class="slot-item">
              <span class="slot-emoji">{{ slot.item.emoji }}</span>
              <span class="slot-name">{{ slot.item.name }}</span>
            </div>
            <div v-else class="empty-slot">⬅️ перетащи сюда</div>
          </div>
        </div>
      </div>
    </div>

    <!-- Доступные элементы (рандомно перемешанные) -->
    <div class="available-items">
      <h3 class="section-title">📦 Доступные элементы</h3>
      <div class="items-container">
        <div
          v-for="item in shuffledAvailableItems"
          :key="item.id"
          class="drag-item"
          :class="{ 'item-selected': isMobile && selectedItem && selectedItem.id === item.id }"
          draggable="true"
          @dragstart="handleDragStart($event, item)"
          @dragend="handleDragEnd"
          @click="isMobile ? selectAvailableItem(item) : null"
        >
          <span class="drag-emoji">{{ item.emoji }}</span>
          <span class="drag-name">{{ item.name }}</span>
        </div>
      </div>
    </div>

    <!-- Правильные цепочки для справки -->
    <div class="reference-box">
      <h4>📖 Правильные цепочки:</h4>
      <div class="reference-chains">
        <div v-for="chain in correctChainExamples" :key="chain.id" class="reference-chain">
          <span>{{ chain.predator }} {{ chain.predatorEmoji }}</span>
          <span>→</span>
          <span>{{ chain.prey }} {{ chain.preyEmoji }}</span>
          <span>→</span>
          <span>{{ chain.waste }} {{ chain.wasteEmoji }}</span>
        </div>
      </div>
    </div>

    <div class="tip-box" v-if="isMobile && selectedItem">
      💡 Выбран: {{ selectedItem.name }}. Теперь нажми на пустую ячейку!
    </div>
    <div class="tip-box" v-else>
      💡 Перетащи элементы в пустые ячейки, чтобы составить правильные пищевые цепочки!
    </div>

    <div v-if="showCompleteModal" class="completion-modal">
      <div class="completion-content">
        <span class="completion-icon">🐬✨</span>
        <h2>Уровень пройден!</h2>
        <p>Ты восстановил пищевые цепочки в океане!</p>
        <p class="reward-text">+250 XP</p>
        <div class="completion-buttons">
          <button class="next-level-btn" @click="goToNextLevel">Следующий уровень →</button>
          <button class="home-btn" @click="goHome">Вернуться домой</button>
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
      required: true,
      default: 2
    }
  },
  data() {
    return {
      currentDragItem: null,
      selectedItem: null,
      isMobile: false,
      mistakes: 0,
      
      // Правильные цепочки (тюлень заменён на более подходящую жертву)
      chains: [
        { id: 1, predator: 'Акула', predatorEmoji: '🦈', prey: 'Скат', preyEmoji: '🪰', waste: 'Кости', wasteEmoji: '🦴' },
        { id: 2, predator: 'Косатка', predatorEmoji: '🐋', prey: 'Рыба', preyEmoji: '🐟', waste: 'Чешуя', wasteEmoji: '✨' },
        { id: 3, predator: 'Мурена', predatorEmoji: '🐍', prey: 'Осьминог', preyEmoji: '🐙', waste: 'Щупальца', wasteEmoji: '🦑' }
      ],
      
      predatorSlots: [
        { id: 1, chainId: 1, type: 'predator', item: null, filled: false, correct: false, wrong: false },
        { id: 2, chainId: 2, type: 'predator', item: null, filled: false, correct: false, wrong: false },
        { id: 3, chainId: 3, type: 'predator', item: null, filled: false, correct: false, wrong: false }
      ],
      
      preySlots: [
        { id: 1, chainId: 1, type: 'prey', item: null, filled: false, correct: false, wrong: false },
        { id: 2, chainId: 2, type: 'prey', item: null, filled: false, correct: false, wrong: false },
        { id: 3, chainId: 3, type: 'prey', item: null, filled: false, correct: false, wrong: false }
      ],
      
      wasteSlots: [
        { id: 1, chainId: 1, type: 'waste', item: null, filled: false, correct: false, wrong: false },
        { id: 2, chainId: 2, type: 'waste', item: null, filled: false, correct: false, wrong: false },
        { id: 3, chainId: 3, type: 'waste', item: null, filled: false, correct: false, wrong: false }
      ],
      
      baseAvailableItems: [
        // Хищники
        { id: 1, name: 'Акула', emoji: '🦈', type: 'predator', chainId: 1 },
        { id: 2, name: 'Косатка', emoji: '🐋', type: 'predator', chainId: 2 },
        { id: 3, name: 'Мурена', emoji: '🐍', type: 'predator', chainId: 3 },
        // Жертвы
        { id: 4, name: 'Скат', emoji: '🪰', type: 'prey', chainId: 1 },
        { id: 5, name: 'Рыба', emoji: '🐟', type: 'prey', chainId: 2 },
        { id: 6, name: 'Осьминог', emoji: '🐙', type: 'prey', chainId: 3 },
        // Отходы
        { id: 7, name: 'Кости', emoji: '🦴', type: 'waste', chainId: 1 },
        { id: 8, name: 'Чешуя', emoji: '✨', type: 'waste', chainId: 2 },
        { id: 9, name: 'Щупальца', emoji: '🦑', type: 'waste', chainId: 3 }
      ],
      
      shuffledAvailableItems: [],
      showCompleteModal: false
    }
  },
  
  computed: {
    correctChains() {
      let count = 0
      for (let i = 0; i < this.chains.length; i++) {
        const predatorSlot = this.predatorSlots.find(s => s.chainId === i + 1)
        const preySlot = this.preySlots.find(s => s.chainId === i + 1)
        const wasteSlot = this.wasteSlots.find(s => s.chainId === i + 1)
        if (predatorSlot?.correct && preySlot?.correct && wasteSlot?.correct) {
          count++
        }
      }
      return count
    },
    
    totalChains() {
      return this.chains.length
    },
    
    isComplete() {
      return this.correctChains === this.totalChains && !this.showCompleteModal
    },
    
    correctChainExamples() {
      return this.chains
    }
  },
  
  mounted() {
    this.checkMobile()
    this.shuffleAvailableItems()
    window.addEventListener('resize', this.checkMobile)
  },
  
  beforeDestroy() {
    window.removeEventListener('resize', this.checkMobile)
  },
  
  watch: {
    isComplete(val) {
      if (val && !this.showCompleteModal) {
        this.completeLevel()
      }
    }
  },
  
  methods: {
    shuffleArray(array) {
      const shuffled = [...array]
      for (let i = shuffled.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1))
        ;[shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]]
      }
      return shuffled
    },
    
    shuffleAvailableItems() {
      this.shuffledAvailableItems = this.shuffleArray(this.baseAvailableItems)
    },
    
    checkMobile() {
      this.isMobile = window.innerWidth <= 768 || 'ontouchstart' in window
    },
    
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
    
    handleDrop(event, slot, slotType) {
      event.preventDefault()
      if (!this.currentDragItem) return
      this.processPlacement(this.currentDragItem, slot, slotType)
      this.currentDragItem = null
    },
    
    selectAvailableItem(item) {
      if (this.selectedItem && this.selectedItem.id === item.id) {
        this.selectedItem = null
      } else {
        this.selectedItem = item
      }
    },
    
    selectSlot(slot, slotType) {
      if (!this.selectedItem) {
        this.showNoSelectionFeedback()
        return
      }
      this.processPlacement(this.selectedItem, slot, slotType)
      this.selectedItem = null
    },
    
    processPlacement(item, slot, slotType) {
      if (item.type !== slotType) {
        this.showWrongFeedback(slot)
        return
      }
      
      if (item.chainId !== slot.chainId) {
        this.showWrongFeedback(slot)
        return
      }
      
      if (slot.filled) {
        this.showSlotOccupiedFeedback(slot)
        return
      }
      
      slot.item = { ...item }
      slot.filled = true
      slot.correct = true
      this.showCorrectFeedback(slot)
      
      const itemIndex = this.shuffledAvailableItems.findIndex(i => i.id === item.id)
      if (itemIndex !== -1) {
        this.shuffledAvailableItems.splice(itemIndex, 1)
      }
    },
    
    showNoSelectionFeedback() {
      const container = document.querySelector('.available-items')
      container.classList.add('wrong-shake')
      setTimeout(() => container.classList.remove('wrong-shake'), 500)
    },
    
    showWrongFeedback(slot) {
      slot.wrong = true
      this.mistakes++
      setTimeout(() => {
        slot.wrong = false
      }, 500)
    },
    
    showSlotOccupiedFeedback(slot) {
      slot.wrong = true
      setTimeout(() => {
        slot.wrong = false
      }, 500)
    },
    
    showCorrectFeedback(slot) {
      const element = document.querySelector(`.chain-slot[data-id="${slot.id}"]`)
      if (element) {
        element.classList.add('correct-flash')
        setTimeout(() => element.classList.remove('correct-flash'), 500)
      }
    },
    
    completeLevel() {
      this.showCompleteModal = true
      
      const completedLevels = JSON.parse(localStorage.getItem('completedLevels') || '[]')
      if (!completedLevels.includes(this.levelId)) {
        completedLevels.push(this.levelId)
        localStorage.setItem('completedLevels', JSON.stringify(completedLevels))
      }
      
      const userXP = parseInt(localStorage.getItem('userXP') || '0')
      localStorage.setItem('userXP', userXP + 250)
    },
    
    goToNextLevel() {
      this.$emit('complete', this.levelId)
      this.$emit('go-home')
    },
    
    goHome() {
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
  50% { border-color: #4caf50; background: rgba(76, 175, 80, 0.3); box-shadow: 0 0 10px #4caf50; }
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
  margin-bottom: 20px;
  flex-wrap: wrap;
  gap: 10px;
  position: relative;
  z-index: 2;
}

.level-badge, .score-card, .mistake-card {
  background: rgba(27, 51, 79, 0.8);
  backdrop-filter: blur(10px);
  padding: 8px 16px;
  border-radius: 40px;
  border: 1px solid rgba(64, 224, 208, 0.3);
  display: flex;
  align-items: center;
  gap: 8px;
}

.level-icon, .score-icon, .mistake-icon {
  font-size: 20px;
}

.level-title, .score-text, .mistake-text {
  font-size: 14px;
  font-weight: bold;
  color: white;
}

.score-text, .mistake-text {
  color: #ffd700;
  font-size: 18px;
}

.score-label, .mistake-label {
  color: white;
  font-size: 12px;
}

.game-layout {
  display: flex;
  align-items: stretch;
  justify-content: center;
  gap: 15px;
  margin-bottom: 30px;
  flex-wrap: wrap;
}

.chain-column {
  flex: 1;
  min-width: 180px;
  background: rgba(4, 33, 69, 0.5);
  backdrop-filter: blur(5px);
  border-radius: 20px;
  padding: 15px;
  border: 1px solid rgba(64, 224, 208, 0.3);
}

.column-header {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  margin-bottom: 15px;
  padding-bottom: 10px;
  border-bottom: 1px solid rgba(64, 224, 208, 0.3);
}

.column-header h3 {
  color: white;
  font-size: 16px;
  margin: 0;
}

.column-icon {
  font-size: 24px;
}

.chain-arrow {
  font-size: 40px;
  color: rgba(64, 224, 208, 0.6);
  display: flex;
  align-items: center;
  padding: 0 5px;
}

.column-content {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.chain-slot {
  min-height: 70px;
  background: rgba(255, 255, 255, 0.05);
  border: 2px dashed rgba(64, 224, 208, 0.3);
  border-radius: 15px;
  transition: all 0.2s;
  cursor: pointer;
}

.chain-slot:hover {
  border-color: #40e0d0;
  background: rgba(255, 255, 255, 0.1);
}

.chain-slot.filled {
  border: 2px solid rgba(76, 175, 80, 0.5);
  background: rgba(76, 175, 80, 0.1);
}

.chain-slot.correct {
  border-color: #4caf50;
  background: rgba(76, 175, 80, 0.15);
}

.chain-slot.wrong {
  border-color: #f44336;
  background: rgba(244, 67, 54, 0.15);
  animation: wrongShake 0.5s ease;
}

.slot-item {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  padding: 12px;
}

.slot-emoji {
  font-size: 32px;
}

.slot-name {
  color: white;
  font-size: 14px;
  font-weight: 500;
}

.empty-slot {
  color: rgba(255, 255, 255, 0.4);
  font-size: 12px;
  text-align: center;
  padding: 20px;
}

.available-items {
  background: rgba(27, 51, 79, 0.8);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  padding: 15px;
  margin-bottom: 15px;
}

.section-title {
  color: white;
  font-size: 16px;
  margin-bottom: 15px;
  text-align: center;
}

.items-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 12px;
}

.drag-item {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 15px;
  padding: 8px 16px;
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: grab;
  transition: all 0.2s;
  border: 1px solid rgba(64, 224, 208, 0.3);
}

.drag-item:active {
  cursor: grabbing;
}

.drag-item:hover {
  transform: translateY(-3px);
  background: rgba(255, 255, 255, 0.2);
}

.drag-emoji {
  font-size: 28px;
}

.drag-name {
  color: white;
  font-size: 14px;
}

.item-selected {
  background: rgba(50, 180, 144, 0.4);
  border-color: #ffd700;
  transform: scale(1.02);
}

.reference-box {
  background: rgba(0, 0, 0, 0.4);
  border-radius: 15px;
  padding: 10px 15px;
  margin-bottom: 15px;
}

.reference-box h4 {
  color: #ffd700;
  font-size: 12px;
  margin: 0 0 8px 0;
}

.reference-chains {
  display: flex;
  flex-wrap: wrap;
  gap: 15px;
  justify-content: center;
}

.reference-chain {
  display: flex;
  align-items: center;
  gap: 5px;
  font-size: 12px;
  color: white;
  background: rgba(255, 255, 255, 0.1);
  padding: 4px 8px;
  border-radius: 20px;
}

.tip-box {
  background: rgba(0, 0, 0, 0.5);
  border-radius: 15px;
  padding: 10px 15px;
  color: #ffd700;
  font-size: 12px;
  text-align: center;
}

.wrong-shake {
  animation: wrongShake 0.5s ease;
}

.correct-flash {
  animation: correctFlash 0.5s ease;
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
  padding: 30px;
  border-radius: 30px;
  text-align: center;
  max-width: 400px;
  border: 2px solid #40e0d0;
  animation: slideUp 0.5s;
}

.completion-icon {
  font-size: 56px;
  animation: gentleFloat 2s infinite;
}

.completion-content h2 {
  color: #ffd700;
  font-size: 28px;
  margin: 15px 0;
}

.completion-content p {
  color: white;
  font-size: 16px;
  margin: 8px 0;
}

.reward-text {
  font-size: 24px !important;
  color: #ffd700 !important;
  font-weight: bold;
  margin: 15px 0 !important;
}

.completion-buttons {
  display: flex;
  gap: 12px;
  justify-content: center;
  margin-top: 15px;
  flex-wrap: wrap;
}

.next-level-btn, .home-btn {
  padding: 10px 20px;
  border-radius: 40px;
  font-size: 14px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s;
}

.next-level-btn {
  background: linear-gradient(135deg, #40e0d0, #008080);
  color: white;
  border: none;
}

.home-btn {
  background: rgba(27, 51, 79, 0.8);
  color: white;
  border: 1px solid #40e0d0;
}

.next-level-btn:hover, .home-btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
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
    padding: 70px 12px 12px 12px;
  }
  
  .game-layout {
    flex-direction: column;
    align-items: center;
  }
  
  .chain-arrow {
    transform: rotate(90deg);
    padding: 5px 0;
  }
  
  .chain-column {
    width: 100%;
    min-width: auto;
  }
  
  .slot-emoji {
    font-size: 24px;
  }
  
  .slot-name, .drag-name {
    font-size: 12px;
  }
  
  .drag-emoji {
    font-size: 24px;
  }
  
  .reference-chain {
    font-size: 10px;
  }
}
</style>