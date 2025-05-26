<template>
  <main class="test-page">
    <div class="test-container">
      <div v-if="result === null" class="test-content">
        <div class="test-header">
          <div class="progress-bar">
            <div class="progress" :style="{ width: progress + '%' }"></div>
          </div>
          <div class="question-category">{{ questions[currentQuestion].category }}</div>
        </div>
        
        <div class="question-block">
          <h2>Вопрос {{ currentQuestion + 1 }} из {{ questions.length }}</h2>
          <p class="question-text">{{ questions[currentQuestion].question }}</p>
          
          <div v-if="questions[currentQuestion].type === 'text'" class="answer-input">
            <input v-model="userInput" placeholder="Введите ваш ответ" />
          </div>
          
          <div v-else class="options-list">
            <div v-for="opt in questions[currentQuestion].options" 
                 :key="opt" 
                 class="option-item"
                 :class="{ selected: userInput === opt }"
                 @click="userInput = opt">
              <input type="radio" :value="opt" v-model="userInput" :id="opt" />
              <label :for="opt">{{ opt }}</label>
            </div>
          </div>
        </div>

        <div class="test-controls">
          <button @click="prev" :disabled="currentQuestion === 0" class="control-btn prev-btn">
            Назад
          </button>
          <button @click="skipQuestion" class="control-btn skip-btn">
            Пропустить
          </button>
          <button @click="next" class="control-btn next-btn">
            Далее
          </button>
        </div>
      </div>

      <div v-else class="result-block">
        <h2>Результаты теста</h2>
        <div class="result-stats">
          <div class="stat-item">
            <span class="stat-label">Общий балл:</span>
            <span class="stat-value">{{ result.score }}%</span>
          </div>
          <div class="stat-item">
            <span class="stat-label">Правильных ответов:</span>
            <span class="stat-value">{{ result.correctAnswers }} из {{ result.totalQuestions }}</span>
          </div>
          <div class="stat-item">
            <span class="stat-label">Пропущено вопросов:</span>
            <span class="stat-value">{{ result.skippedQuestions }}</span>
          </div>
        </div>
        <button @click="restartTest" class="restart-btn">Пройти тест снова</button>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  name: 'TestComponent',
  data() {
    return {
      currentQuestion: 0,
      answers: [],
      skippedQuestions: [],
      questions: [
        { type: 'text', category: 'География', question: 'Какая столица Франции?', answer: 'Париж', points: 10 },
        { type: 'choice', category: 'Математика', question: '2 + 2 = ?', options: ['3', '4', '5'], answer: '4', points: 5 },
        { type: 'text', category: 'Литература', question: 'Кто написал \"Гамлета\"?', answer: 'Шекспир', points: 10 },
        { type: 'choice', category: 'Астрономия', question: 'Самая большая планета?', options: ['Марс', 'Юпитер', 'Венера'], answer: 'Юпитер', points: 5 },
        { type: 'text', category: 'Информатика', question: 'Что означает HTTP?', answer: 'HyperText Transfer Protocol', points: 10 },
        { type: 'choice', category: 'Информатика', question: 'HTML это ...?', options: ['Язык', 'Протокол', 'Устройство'], answer: 'Язык', points: 5 },
        { type: 'text', category: 'География', question: 'Самая маленькая страна?', answer: 'Ватикан', points: 10 },
        { type: 'text', category: 'Матемитика', question: 'Корень квадратный из 169?', answer: '13', points: 10 },
        { type: 'choice', category: 'Лиетратура', question: 'Кто написал \"Мастер и Маргарита\"?', options: ['Булгаков', 'Достоевский', 'Толстой'], answer: 'Булгаков', points: 5 },
        { type: 'choice', category: 'Политика', question: 'В каком году умер Папа Римский Франциск?', options: ['1978', '1983', '2025'], answer: '2025', points: 5 },
        { type: 'choice', category: 'Литература и Музыка', question: 'Кто такая Агата Кристи?', options: ['Писательница', 'Магазин', 'Спортсменка', 'Навзвание газеты'], answer: 'Писательница', points: 10 },
        { type: 'text', category: 'Спорт', question: 'За какую сборную играет Леонель Месси?', answer: 'Аргентина', points: 10 }
      ],
      result: null,
      userInput: ''
    }
  },
  computed: {
    progress() {
      return (this.currentQuestion / this.questions.length) * 100
    }
  },
  methods: {
    next() {
      if (this.userInput || this.skippedQuestions.includes(this.currentQuestion)) {
        this.answers[this.currentQuestion] = this.userInput
        this.userInput = ''
        this.currentQuestion++
        if (this.currentQuestion < this.questions.length) {
          // продолжаем тест
        } else {
          this.calculateResult()
        }
      }
    },
    prev() {
      if (this.currentQuestion > 0) {
        this.currentQuestion--
        this.userInput = this.answers[this.currentQuestion] || ''
      }
    },
    skipQuestion() {
      if (!this.skippedQuestions.includes(this.currentQuestion)) {
        this.skippedQuestions.push(this.currentQuestion)
        this.userInput = ''
        this.next()
      }
    },
    calculateResult() {
      let score = 0
      let totalPoints = 0
      let correctAnswers = 0
      
      this.questions.forEach((q, index) => {
        totalPoints += q.points
        if (!this.skippedQuestions.includes(index)) {
          if ((this.answers[index] || '').toLowerCase().trim() === q.answer.toLowerCase().trim()) {
            score += q.points
            correctAnswers++
          }
        }
      })
      
      this.result = {
        score: Math.round((score / totalPoints) * 100),
        correctAnswers,
        totalQuestions: this.questions.length,
        skippedQuestions: this.skippedQuestions.length
      }
    },
    restartTest() {
      this.currentQuestion = 0
      this.answers = []
      this.skippedQuestions = []
      this.userInput = ''
      this.result = null
    }
  }
}
</script>