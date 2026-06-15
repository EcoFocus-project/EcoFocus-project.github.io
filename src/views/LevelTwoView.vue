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
        <span class="level-icon">📋</span>
        <span class="level-title">Уровень 2: Знаешь ли ты океан?</span>
      </div>
      <div class="score-card">
        <span class="score-icon">⭐</span>
        <span class="score-text">{{ earnedXP }}</span>
        <span class="score-label">заработано XP</span>
      </div>
      <div class="correct-card">
        <span class="correct-icon">✓</span>
        <span class="correct-text">{{ correctAnswers }} / {{ totalQuestions }}</span>
        <span class="correct-label">верных ответов</span>
      </div>
    </div>

    <div class="quiz-container">
      <div class="question-card" v-if="!isComplete">
        <div class="question-header">
          <span class="question-number">Вопрос {{ currentQuestionIndex + 1 }} из {{ totalQuestions }}</span>
          <span class="question-points">+{{ questionXP }} XP за верный ответ</span>
        </div>
        
        <div class="question-icon">{{ currentQuestion.icon }}</div>
        <h3 class="question-text">{{ currentQuestion.text }}</h3>
        
        <div class="answers-list">
          <button
            v-for="(answer, idx) in currentQuestion.answers"
            :key="idx"
            class="answer-button"
            :class="{ 
              'answer-correct': answered && answer.correct,
              'answer-wrong': answered && selectedAnswer === idx && !answer.correct,
              'answer-disabled': answered
            }"
            :disabled="answered"
            @click="checkAnswer(idx, answer.correct)"
          >
            <span class="answer-letter">{{ String.fromCharCode(65 + idx) }}.</span>
            <span class="answer-text">{{ answer.text }}</span>
            <span v-if="answered && answer.correct" class="answer-mark">✓</span>
            <span v-if="answered && selectedAnswer === idx && !answer.correct" class="answer-mark">✗</span>
          </button>
        </div>
        
        <div class="feedback-message" v-if="answered">
          <div class="feedback-icon">{{ isAnswerCorrect ? '🎉' : '💡' }}</div>
          <div class="feedback-text">
            <strong>{{ isAnswerCorrect ? 'Верно! +' + questionXP + ' XP' : 'К сожалению, неверно' }}</strong>
            <p>{{ currentQuestion.explanation }}</p>
          </div>
        </div>
        
        <button 
          v-if="answered" 
          class="next-button"
          @click="nextQuestion"
        >
          {{ isLastQuestion ? 'Завершить' : 'Следующий вопрос →' }}
        </button>
      </div>

      <div v-if="isComplete && !showCompleteModal" class="result-card">
        <div class="result-icon">🏆</div>
        <h2 class="result-title">Результат</h2>
        <p class="result-score">Правильных ответов: {{ correctAnswers }} из {{ totalQuestions }}</p>
        <p class="result-xp">Заработано XP: {{ earnedXP }} / {{ maxXP }}</p>
        
        <div class="result-feedback" v-if="correctAnswers === totalQuestions">
          <p>🎉 Идеально! Ты настоящий знаток океана! +{{ maxXP }} XP 🎉</p>
        </div>
        <div class="result-feedback" v-else-if="correctAnswers >= totalQuestions - 2">
          <p>🌊 Отлично! Ты хорошо знаешь океан! 🌊</p>
        </div>
        <div class="result-feedback" v-else>
          <p>🐚 Неплохо, но есть куда расти. Почитай про океан и возвращайся! 🐚</p>
        </div>
        
        <div class="completion-buttons">
          <button class="next-level-btn" @click="completeLevel">Получить награду →</button>
          <button class="home-btn" @click="goHome">Вернуться домой</button>
        </div>
      </div>
    </div>

    <div class="ocean-fact">
      <span class="fact-icon">🐟</span>
      <span class="fact-text">Знаешь ли ты? Океаны производят более 50% кислорода на планете!</span>
    </div>

    <div v-if="showCompleteModal" class="completion-modal">
      <div class="completion-content">
        <span class="completion-icon">🐬✨</span>
        <h2>Уровень пройден!</h2>
        <p>Ты узнал больше об океане и его обитателях!</p>
        <p class="reward-text">+{{ earnedXP }} XP</p>
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
      currentQuestionIndex: 0,
      answered: false,
      selectedAnswer: null,
      isAnswerCorrect: false,
      correctAnswers: 0,
      earnedXP: 0,
      questionXP: 50,
      showCompleteModal: false,
      
      questions: [
        {
          icon: '🌊',
          text: 'Сколько процентов поверхности Земли покрыто океанами?',
          answers: [
            { text: '~50%', correct: false },
            { text: '~61%', correct: false },
            { text: '~71%', correct: true },
            { text: '~81%', correct: false }
          ],
          explanation: 'Океаны покрывают около 71% поверхности нашей планеты! Это больше, чем вся суша вместе взятая.'
        },
        {
          icon: '🐋',
          text: 'Какое животное является самым большим на Земле и живёт в океане?',
          answers: [
            { text: 'Синий кит', correct: true },
            { text: 'Кашалот', correct: false },
            { text: 'Косатка', correct: false },
            { text: 'Гигантский кальмар', correct: false }
          ],
          explanation: 'Синий кит — крупнейшее животное на Земле. Его длина может достигать 30 метров, а вес — 150 тонн!'
        },
        {
          icon: '🗑️',
          text: 'Сколько лет разлагается пластиковая бутылка в океане?',
          answers: [
            { text: '50-100 лет', correct: false },
            { text: '200-300 лет', correct: false },
            { text: '450-500 лет', correct: true },
            { text: '1000+ лет', correct: false }
          ],
          explanation: 'Пластиковая бутылка разлагается от 450 до 500 лет! Вот почему так важно перерабатывать пластик.'
        },
        {
          icon: '🪸',
          text: 'Что такое коралловые рифы?',
          answers: [
            { text: 'Подводные горы', correct: false },
            { text: 'Колонии живых организмов', correct: true },
            { text: 'Морские водоросли', correct: false },
            { text: 'Подводные пещеры', correct: false }
          ],
          explanation: 'Коралловые рифы — это колонии крошечных животных — коралловых полипов. Это "морские джунгли", где живёт множество рыб!'
        },
        {
          icon: '🐠',
          text: 'Какой процент морских обитателей обитает именно на коралловых рифах?',
          answers: [
            { text: '~10%', correct: false },
            { text: '~25%', correct: true },
            { text: '~50%', correct: false },
            { text: '~75%', correct: false }
          ],
          explanation: 'Хотя коралловые рифы занимают менее 1% океана, они являются домом для 25% всех морских обитателей!'
        }
      ]
    }
  },
  
  computed: {
    totalQuestions() {
      return this.questions.length
    },
    
    maxXP() {
      return this.totalQuestions * this.questionXP
    },
    
    currentQuestion() {
      return this.questions[this.currentQuestionIndex]
    },
    
    isLastQuestion() {
      return this.currentQuestionIndex === this.totalQuestions - 1
    },
    
    isComplete() {
      return this.currentQuestionIndex >= this.totalQuestions
    }
  },
  
  methods: {
    checkAnswer(selectedIdx, isCorrect) {
      this.answered = true
      this.selectedAnswer = selectedIdx
      this.isAnswerCorrect = isCorrect
      
      if (isCorrect) {
        this.correctAnswers++
        this.earnedXP += this.questionXP
      }
    },
    
    nextQuestion() {
      this.answered = false
      this.selectedAnswer = null
      this.isAnswerCorrect = false
      this.currentQuestionIndex++
    },
    
    completeLevel() {
      this.showCompleteModal = true
      
      const completedLevels = JSON.parse(localStorage.getItem('completedLevels') || '[]')
      if (!completedLevels.includes(this.levelId)) {
        completedLevels.push(this.levelId)
        localStorage.setItem('completedLevels', JSON.stringify(completedLevels))
      }
      
      const userXP = parseInt(localStorage.getItem('userXP') || '0')
      localStorage.setItem('userXP', userXP + this.earnedXP)
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

@keyframes gentleFloat {
  0%, 100% { transform: translateY(0px); }
  50% { transform: translateY(-6px); }
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideUp {
  from { transform: translateY(50px); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
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

.level-badge, .score-card, .correct-card {
  background: rgba(27, 51, 79, 0.8);
  backdrop-filter: blur(10px);
  padding: 12px 20px;
  border-radius: 40px;
  border: 1px solid rgba(64, 224, 208, 0.3);
  display: flex;
  align-items: center;
  gap: 10px;
}

.level-icon, .score-icon, .correct-icon {
  font-size: 24px;
}

.level-title, .score-text, .correct-text {
  font-size: 18px;
  font-weight: bold;
  color: white;
}

.score-text, .correct-text {
  color: #ffd700;
  font-size: 24px;
}

.score-label, .correct-label {
  color: white;
  font-size: 14px;
}

.correct-icon {
  color: #4caf50;
  font-size: 20px;
}

.quiz-container {
  position: relative;
  z-index: 2;
  max-width: 800px;
  margin: 0 auto;
}

.question-card {
  background: rgba(4, 33, 69, 0.8);
  backdrop-filter: blur(10px);
  border-radius: 40px;
  padding: 30px;
  border: 1px solid rgba(64, 224, 208, 0.3);
}

.question-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  padding-bottom: 15px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.question-number {
  color: rgba(255, 255, 255, 0.7);
  font-size: 14px;
}

.question-points {
  color: #ffd700;
  font-size: 14px;
  font-weight: bold;
}

.question-icon {
  font-size: 64px;
  text-align: center;
  margin-bottom: 15px;
  animation: gentleFloat 3s ease-in-out infinite;
}

.question-text {
  color: white;
  font-size: 24px;
  font-weight: 700;
  text-align: center;
  margin-bottom: 30px;
  line-height: 1.3;
}

.answers-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
  margin-bottom: 20px;
}

.answer-button {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 15px 20px;
  background: rgba(255, 255, 255, 0.08);
  border: 2px solid rgba(64, 224, 208, 0.3);
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.2s;
  text-align: left;
}

.answer-button:hover:not(:disabled) {
  background: rgba(64, 224, 208, 0.15);
  border-color: #40e0d0;
  transform: translateX(5px);
}

.answer-letter {
  font-size: 20px;
  font-weight: bold;
  color: #40e0d0;
  min-width: 35px;
}

.answer-text {
  flex: 1;
  color: white;
  font-size: 16px;
}

.answer-mark {
  font-size: 24px;
  min-width: 30px;
  text-align: center;
}

.answer-correct {
  background: rgba(76, 175, 80, 0.2);
  border-color: #4caf50;
}

.answer-wrong {
  background: rgba(244, 67, 54, 0.2);
  border-color: #f44336;
}

.answer-disabled {
  cursor: default;
  opacity: 0.9;
}

.feedback-message {
  display: flex;
  gap: 15px;
  padding: 15px;
  background: rgba(0, 0, 0, 0.5);
  border-radius: 20px;
  margin: 20px 0;
}

.feedback-icon {
  font-size: 32px;
}

.feedback-text {
  flex: 1;
}

.feedback-text strong {
  color: #ffd700;
  font-size: 18px;
}

.feedback-text p {
  color: white;
  font-size: 14px;
  margin: 5px 0 0;
  line-height: 1.4;
}

.next-button {
  width: 100%;
  padding: 15px;
  background: linear-gradient(135deg, #40e0d0, #008080);
  color: white;
  border: none;
  border-radius: 30px;
  font-size: 18px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s;
}

.next-button:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
}

.result-card {
  background: rgba(4, 33, 69, 0.8);
  backdrop-filter: blur(10px);
  border-radius: 40px;
  padding: 40px;
  text-align: center;
  border: 1px solid rgba(64, 224, 208, 0.3);
}

.result-icon {
  font-size: 80px;
  margin-bottom: 20px;
}

.result-title {
  color: white;
  font-size: 36px;
  margin-bottom: 15px;
}

.result-score {
  color: white;
  font-size: 20px;
  margin-bottom: 10px;
}

.result-xp {
  color: #ffd700;
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 20px;
}

.result-feedback p {
  color: white;
  font-size: 18px;
  margin-bottom: 30px;
}

.ocean-fact {
  background: rgba(0, 0, 0, 0.5);
  border-radius: 20px;
  padding: 12px 20px;
  margin-top: 20px;
  display: flex;
  align-items: center;
  gap: 12px;
  position: relative;
  z-index: 2;
}

.fact-icon {
  font-size: 28px;
}

.fact-text {
  color: #ffd700;
  font-size: 14px;
}

.completion-buttons {
  display: flex;
  gap: 15px;
  justify-content: center;
  margin-top: 20px;
  flex-wrap: wrap;
}

.next-level-btn, .home-btn {
  padding: 12px 25px;
  border-radius: 40px;
  font-size: 16px;
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
  max-width: 450px;
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

@media (max-width: 768px) {
  .ocean-level-container {
    padding: 70px 15px 15px 15px;
  }
  
  .level-header {
    flex-direction: column;
    align-items: stretch;
  }
  
  .question-card {
    padding: 20px;
  }
  
  .question-text {
    font-size: 18px;
  }
  
  .answer-button {
    padding: 12px 15px;
  }
  
  .answer-text {
    font-size: 14px;
  }
  
  .level-title {
    font-size: 14px;
  }
  
  .score-text, .correct-text {
    font-size: 18px;
  }
  
  .result-title {
    font-size: 28px;
  }
  
  .result-score, .result-xp {
    font-size: 16px;
  }
  
  .result-feedback p {
    font-size: 16px;
  }
}
</style>