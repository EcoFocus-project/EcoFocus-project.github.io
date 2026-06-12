<template>
  <div class="ocean-level-container">
    <!-- Анимированные волны на фоне -->
    <div class="waves">
      <div class="wave wave1"></div>
      <div class="wave wave2"></div>
      <div class="wave wave3"></div>
    </div>
    
    <!-- Плавающие пузырьки -->
    <div class="bubbles">
      <div v-for="i in 15" :key="i" class="bubble" :style="{
        left: Math.random() * 100 + '%',
        animationDelay: Math.random() * 5 + 's',
        animationDuration: 3 + Math.random() * 4 + 's'
      }"></div>
    </div>

    <!-- Кнопка назад - исправлена -->
    <div class="top-bar">
      <button class="back-button" @click="goBackToOcean">
        ← Назад
      </button>
    </div>

    <div class="level-header">
      <div class="level-badge">
        <span class="level-icon">🌊</span>
        <span class="level-title">Друг океанов</span>
      </div>
      <div class="score-card">
        <span class="score-icon">⭐</span>
        <span class="score-text">{{ score }} / {{ totalItems }}</span>
        <span class="score-label">предметов спасено</span>
      </div>
    </div>

    <div class="eco-message">
      <p class="message-text">
        🌊 Океан в опасности! Помоги рассортировать мусор и спасти его обитателей!
      </p>
    </div>

    <!-- Перетаскиваемые предметы -->
    <div class="items-container">
      <h3 class="section-title">
        <span class="title-icon">🗑️</span>
        Предметы, которые нужно убрать из океана:
      </h3>
      <div class="drag-items">
        <div
          v-for="item in dragItems"
          :key="item.id"
          class="drag-item"
          :class="getItemClass(item.category)"
          :data-item-id="item.id"
          :data-category="item.category"
          draggable="true"
          @dragstart="handleDragStart"
          @dragend="handleDragEnd"
        >
          <span class="item-icon">{{ getItemIcon(item.name) }}</span>
          <span class="item-name">{{ item.name }}</span>
        </div>
      </div>
      
      <div v-if="dragItems.length === 0" class="success-message">
        <span class="success-icon">🐬</span>
        <p>Отлично! Океан становится чище!</p>
      </div>
    </div>

    <!-- Области для сортировки -->
    <div class="drop-zones">
      <div
        v-for="zone in dropZones"
        :key="zone.category"
        class="drop-zone"
        :class="{
          'drop-zone-correct': zone.isCorrectDrop,
          'drop-zone-wrong': zone.isWrongDrop,
          [zone.category]: true
        }"
        :data-zone-category="zone.category"
        @dragover.prevent
        @drop="handleDrop"
      >
        <div class="zone-header">
          <span class="zone-icon">{{ getZoneIcon(zone.category) }}</span>
          <h3>{{ zone.title }}</h3>
        </div>
        <div class="placed-items">
          <div
            v-for="item in getItemsInZone(zone.category)"
            :key="item.id"
            class="placed-item"
          >
            <span class="item-icon">{{ getItemIcon(item.name) }}</span>
            <span>{{ item.name }}</span>
          </div>
        </div>
        <div class="zone-info">
          <span class="info-text">{{ getZoneInfo(zone.category) }}</span>
        </div>
      </div>
    </div>

    <div v-if="isComplete" class="completion-modal">
      <div class="completion-content">
        <span class="completion-icon">🐬✨</span>
        <h2>Поздравляю!</h2>
        <p>Ты помог очистить океан и спасти его обитателей!</p>
        <p class="reward-text">+250 XP</p>
        <button class="continue-btn" @click="continueToNext">
          Продолжить путешествие →
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LevelView',
  props: {
    userMode: {
      type: String,
      required: true
    },
    levelId: {
      type: Number,
      default: 1
    }
  },
  data() {
    return {
      // Эко-предметы для сортировки
      items: [
        { id: 1, name: 'Пластиковая бутылка', category: 'plastic', eco: true },
        { id: 2, name: 'Стеклянная банка', category: 'glass', eco: true },
        { id: 3, name: 'Бумажный пакет', category: 'paper', eco: true },
        { id: 4, name: 'Алюминиевая банка', category: 'metal', eco: true },
        { id: 5, name: 'Пластиковый пакет', category: 'plastic', eco: true },
        { id: 6, name: 'Рыболовная сеть', category: 'plastic', eco: true },
        { id: 7, name: 'Батарейка', category: 'hazardous', eco: true },
        { id: 8, name: 'ПЭТ-бутылка', category: 'plastic', eco: true },
        { id: 9, name: 'Стеклянная бутылка', category: 'glass', eco: true }
      ],
      
      // Зоны для переработки
      dropZones: [
        { 
          category: 'plastic', 
          title: 'Пластик', 
          items: [], 
          isCorrectDrop: false, 
          isWrongDrop: false,
          color: '#4fc3f7'
        },
        { 
          category: 'glass', 
          title: 'Стекло', 
          items: [], 
          isCorrectDrop: false, 
          isWrongDrop: false,
          color: '#81c784'
        },
        { 
          category: 'paper', 
          title: 'Бумага', 
          items: [], 
          isCorrectDrop: false, 
          isWrongDrop: false,
          color: '#ffb74d'
        },
        { 
          category: 'metal', 
          title: 'Металл', 
          items: [], 
          isCorrectDrop: false, 
          isWrongDrop: false,
          color: '#e0e0e0'
        },
        { 
          category: 'hazardous', 
          title: 'Опасные отходы', 
          items: [], 
          isCorrectDrop: false, 
          isWrongDrop: false,
          color: '#ef5350'
        }
      ],
      
      currentDragItem: null,
      showCompletion: false
    }
  },
  
  computed: {
    dragItems() {
      const placedIds = this.dropZones.flatMap(zone => zone.items.map(item => item.id))
      return this.items.filter(item => !placedIds.includes(item.id))
    },
    
    totalItems() {
      return this.items.length
    },
    
    score() {
      return this.dropZones.reduce((total, zone) => total + zone.items.length, 0)
    },
    
    isComplete() {
      return this.score === this.totalItems && !this.showCompletion
    }
  },
  
  watch: {
    isComplete(val) {
      if (val) {
        this.showCompletion = true
      }
    }
  },
  
  methods: {
    // Новая функция для возврата к OceanIntroView
    goBackToOcean() {
      this.$emit('go-home')
    },
    
    getItemIcon(itemName) {
      const icons = {
        'Пластиковая бутылка': '🧴',
        'Пластиковый пакет': '🛍️',
        'Рыболовная сеть': '🎣',
        'ПЭТ-бутылка': '🧴',
        'Стеклянная банка': '🥫',
        'Стеклянная бутылка': '🍾',
        'Бумажный пакет': '📦',
        'Алюминиевая банка': '🥤',
        'Батарейка': '🔋'
      }
      return icons[itemName] || '🗑️'
    },
    
    getZoneIcon(category) {
      const icons = {
        plastic: '🧴',
        glass: '🥫',
        paper: '📄',
        metal: '🥤',
        hazardous: '⚠️'
      }
      return icons[category] || '♻️'
    },
    
    getZoneInfo(category) {
      const info = {
        plastic: 'Перерабатывается в новые бутылки и одежду',
        glass: 'Может переплавляться бесконечно',
        paper: 'Спасает деревья от вырубки',
        metal: 'Экономит 95% энергии при переработке',
        hazardous: 'Требует специальной утилизации'
      }
      return info[category] || 'Помоги океану!'
    },
    
    getItemClass(category) {
      return {
        'item-plastic': category === 'plastic',
        'item-glass': category === 'glass',
        'item-paper': category === 'paper',
        'item-metal': category === 'metal',
        'item-hazardous': category === 'hazardous'
      }
    },
    
    handleDragStart(event) {
      const itemId = parseInt(event.target.closest('.drag-item').dataset.itemId)
      this.currentDragItem = this.items.find(item => item.id === itemId)
      
      event.dataTransfer.setData('text/plain', JSON.stringify(this.currentDragItem))
      event.dataTransfer.effectAllowed = 'move'
      
      event.target.closest('.drag-item').classList.add('dragging')
    },
    
    handleDragEnd(event) {
      event.target.closest('.drag-item')?.classList.remove('dragging')
      this.currentDragItem = null
      
      this.dropZones.forEach(zone => {
        zone.isCorrectDrop = false
        zone.isWrongDrop = false
      })
    },
    
    handleDrop(event) {
      event.preventDefault()
      
      if (!this.currentDragItem) return
      
      const zoneCategory = event.currentTarget.dataset.zoneCategory
      const targetZone = this.dropZones.find(zone => zone.category === zoneCategory)
      
      const isCorrect = this.currentDragItem.category === zoneCategory
      
      if (isCorrect) {
        targetZone.items.push({ ...this.currentDragItem })
        targetZone.isCorrectDrop = true
        
        // Визуальный эффект пузырьков при правильной сортировке
        this.createBubbleEffect(event)
        
        setTimeout(() => {
          targetZone.isCorrectDrop = false
        }, 500)
      } else {
        targetZone.isWrongDrop = true
        
        // Анимация ошибки
        const dragItem = document.querySelector(`[data-item-id="${this.currentDragItem.id}"]`)
        if (dragItem) {
          dragItem.classList.add('shake')
          setTimeout(() => {
            dragItem.classList.remove('shake')
          }, 500)
        }
        
        setTimeout(() => {
          targetZone.isWrongDrop = false
        }, 500)
      }
      
      this.currentDragItem = null
    },
    
    createBubbleEffect(event) {
      const rect = event.currentTarget.getBoundingClientRect()
      const bubble = document.createElement('div')
      bubble.className = 'effect-bubble'
      bubble.style.left = (event.clientX - rect.left) + 'px'
      bubble.style.top = (event.clientY - rect.top) + 'px'
      event.currentTarget.appendChild(bubble)
      setTimeout(() => bubble.remove(), 500)
    },
    
    getItemsInZone(category) {
      const zone = this.dropZones.find(zone => zone.category === category)
      return zone ? zone.items : []
    },
    
    continueToNext() {
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

@keyframes bubble {
  0% { transform: translateY(0) scale(0); opacity: 0; }
  50% { opacity: 1; }
  100% { transform: translateY(-100vh) scale(1); opacity: 0; }
}

@keyframes float {
  0%, 100% { transform: translateY(0px); }
  50% { transform: translateY(-10px); }
}

@keyframes pulse {
  0%, 100% { transform: scale(1); opacity: 0.8; }
  50% { transform: scale(1.05); opacity: 1; }
}

.ocean-level-container {
  min-height: 100vh;
  background: linear-gradient(135deg, #0c376a 0%, #042145 50%, #01152b 100%);
  padding: 80px 20px 20px 20px; /* Добавил отступ сверху, чтобы контент не налезал на кнопку */
  position: relative;
  overflow-x: hidden;
}

/* Верхняя панель с кнопкой */
.top-bar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  padding: 15px 20px;
  background: rgba(4, 33, 69, 0.95);
  backdrop-filter: blur(10px);
  z-index: 100;
  border-bottom: 1px solid rgba(64, 224, 208, 0.3);
}

.back-button {
  padding: 10px 24px;
  background: linear-gradient(135deg, #1b5e8c, #0c376a);
  color: white;
  border: 1px solid rgba(64, 224, 208, 0.5);
  border-radius: 30px;
  cursor: pointer;
  font-size: 18px;
  font-weight: bold;
  transition: all 0.3s;
  width: fit-content;
}

.back-button:hover {
  background: linear-gradient(135deg, #0c376a, #042145);
  transform: translateX(-5px);
  border-color: #40e0d0;
}

/* Волны */
.waves {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 200px;
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

/* Пузырьки */
.bubbles {
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  pointer-events: none;
  z-index: 0;
}

.bubble {
  position: absolute;
  bottom: -50px;
  width: 20px;
  height: 20px;
  background: radial-gradient(circle, rgba(255,255,255,0.8), rgba(255,255,255,0.2));
  border-radius: 50%;
  animation: bubble 8s infinite ease-in;
  pointer-events: none;
}

/* Основной контент */
.level-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 30px;
  position: relative;
  z-index: 1;
  flex-wrap: wrap;
  gap: 15px;
}

.level-badge {
  background: linear-gradient(135deg, #1b5e8c, #0c376a);
  padding: 15px 25px;
  border-radius: 50px;
  display: flex;
  align-items: center;
  gap: 15px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.3);
  border: 1px solid rgba(64, 224, 208, 0.5);
}

.level-icon {
  font-size: 32px;
  animation: float 3s infinite;
}

.level-title {
  font-size: 24px;
  font-weight: bold;
  color: white;
  margin: 0;
}

.score-card {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  padding: 15px 25px;
  border-radius: 50px;
  display: flex;
  align-items: center;
  gap: 12px;
  border: 1px solid rgba(64, 224, 208, 0.5);
}

.score-icon {
  font-size: 28px;
}

.score-text {
  font-size: 28px;
  font-weight: bold;
  color: #ffd700;
}

.score-label {
  color: white;
  font-size: 14px;
}

.eco-message {
  background: linear-gradient(135deg, rgba(27, 94, 140, 0.8), rgba(4, 33, 69, 0.8));
  backdrop-filter: blur(10px);
  border-left: 4px solid #40e0d0;
  padding: 15px 20px;
  margin-bottom: 30px;
  border-radius: 10px;
  position: relative;
  z-index: 1;
}

.message-text {
  margin: 0;
  color: white;
  font-size: 18px;
  font-weight: 500;
}

.items-container {
  background: rgba(255,255,255,0.08);
  backdrop-filter: blur(10px);
  padding: 25px;
  border-radius: 20px;
  margin-bottom: 30px;
  position: relative;
  z-index: 1;
  border: 1px solid rgba(64, 224, 208, 0.3);
}

.section-title {
  color: white;
  font-size: 22px;
  margin-bottom: 20px;
  display: flex;
  align-items: center;
  gap: 10px;
}

.title-icon {
  font-size: 28px;
}

.drag-items {
  display: flex;
  flex-wrap: wrap;
  gap: 15px;
  justify-content: center;
}

.drag-item {
  background: linear-gradient(135deg, #1b5e8c, #0c376a);
  color: white;
  padding: 12px 20px;
  border-radius: 12px;
  cursor: grab;
  user-select: none;
  transition: all 0.3s;
  font-size: 16px;
  font-weight: 500;
  display: flex;
  align-items: center;
  gap: 10px;
  border: 2px solid rgba(64, 224, 208, 0.3);
  box-shadow: 0 4px 15px rgba(0,0,0,0.2);
}

.drag-item:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 25px rgba(0,0,0,0.3);
  border-color: #40e0d0;
}

.drag-item.dragging {
  opacity: 0.5;
  cursor: grabbing;
}

.item-icon {
  font-size: 24px;
}

.item-name {
  font-size: 14px;
  font-weight: 500;
}

/* Цвета для разных типов предметов */
.item-plastic { background: linear-gradient(135deg, #4fc3f7, #0288d1); }
.item-glass { background: linear-gradient(135deg, #81c784, #388e3c); }
.item-paper { background: linear-gradient(135deg, #ffb74d, #f57c00); }
.item-metal { background: linear-gradient(135deg, #e0e0e0, #9e9e9e); color: #333; }
.item-hazardous { background: linear-gradient(135deg, #ef5350, #c62828); }

.drop-zones {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 20px;
  position: relative;
  z-index: 1;
  margin-top: 20px;
}

.drop-zone {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border: 3px dashed rgba(64, 224, 208, 0.5);
  border-radius: 20px;
  padding: 20px;
  min-height: 250px;
  transition: all 0.3s;
  position: relative;
  overflow: hidden;
}

.drop-zone:hover {
  border-color: #40e0d0;
  background: rgba(255,255,255,0.15);
  transform: translateY(-5px);
}

.drop-zone-correct {
  border-color: #4caf50 !important;
  background: rgba(76, 175, 80, 0.2) !important;
  animation: pulse 0.5s ease;
}

.drop-zone-wrong {
  border-color: #f44336 !important;
  background: rgba(244, 67, 54, 0.2) !important;
  animation: shake 0.5s ease;
}

.zone-header {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 15px;
  color: white;
}

.zone-header h3 {
  margin: 0;
  font-size: 20px;
}

.zone-icon {
  font-size: 30px;
}

.placed-items {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-top: 15px;
  max-height: 150px;
  overflow-y: auto;
}

.placed-item {
  background: rgba(64, 224, 208, 0.2);
  padding: 6px 12px;
  border-radius: 8px;
  font-size: 12px;
  color: white;
  display: flex;
  align-items: center;
  gap: 5px;
  border: 1px solid rgba(64, 224, 208, 0.5);
}

.zone-info {
  margin-top: 15px;
  font-size: 11px;
  color: rgba(255,255,255,0.6);
  text-align: center;
  padding-top: 10px;
  border-top: 1px solid rgba(255,255,255,0.1);
}

.success-message {
  text-align: center;
  padding: 20px;
  background: linear-gradient(135deg, rgba(76, 175, 80, 0.2), rgba(64, 224, 208, 0.2));
  border-radius: 15px;
  margin-top: 20px;
}

.success-icon {
  font-size: 48px;
  animation: float 2s infinite;
}

.success-message p {
  color: white;
  font-size: 18px;
  margin: 10px 0 0;
}

/* Модальное окно завершения */
.completion-modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0,0,0,0.8);
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
  animation: float 2s infinite;
}

.completion-content h2 {
  color: #ffd700;
  font-size: 36px;
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

.continue-btn {
  background: linear-gradient(135deg, #40e0d0, #008080);
  color: white;
  border: none;
  padding: 15px 30px;
  border-radius: 50px;
  font-size: 18px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s;
  margin-top: 20px;
}

.continue-btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 25px rgba(0,0,0,0.3);
}

/* Эффекты */
@keyframes shake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-5px); }
  75% { transform: translateX(5px); }
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideUp {
  from { transform: translateY(50px); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}

.effect-bubble {
  position: absolute;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(64,224,208,0.8), rgba(64,224,208,0));
  pointer-events: none;
  animation: bubble 0.5s ease-out forwards;
}

/* Адаптивность */
@media (max-width: 768px) {
  .ocean-level-container {
    padding: 70px 15px 15px 15px;
  }
  
  .level-header {
    flex-direction: column;
    align-items: stretch;
  }
  
  .level-badge, .score-card {
    justify-content: center;
  }
  
  .drop-zones {
    grid-template-columns: 1fr;
  }
  
  .level-title {
    font-size: 18px;
  }
  
  .drag-item {
    font-size: 12px;
    padding: 8px 12px;
  }
  
  .top-bar {
    padding: 10px 15px;
  }
  
  .back-button {
    font-size: 16px;
    padding: 8px 20px;
  }
}
</style>