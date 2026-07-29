<template>
  <div class="present-simple-container">
    <!-- Mnemonic poem -->
    <img class="mnemonic-poem" src="./images/mnemonic-poem.jpg" alt="Present simple mnemonic poem">

    <!-- Algorithm Section -->
    <div class="card">
      <h2>Пошаговый алгоритм</h2>
      <ol class="step-list">
        <li>
          <strong>Подлежащее He / She / It?</strong>
          <div>&bull; <em>Нет:</em> Окончание <strong>не меняется</strong> (work, study, watch).</div>
          <div>&bull; <em>Да:</em> Переходим к шагу 2.</div>
        </li>
        <li>
          <strong>Глагол заканчивается на согласный + Y?</strong>
          <div>&bull; <em>Да:</em> Добавляем <strong>-IES</strong> (cry &rarr; cries).</div>
          <div>&bull; <em>Нет:</em> Переходим к шагу 3.</div>
        </li>
        <li>
          <strong>Глагол заканчивается на шипящий (сh, sh), Z или S.O.X?</strong>
          <div>&bull; <em>Да:</em> Добавляем <strong>-ES</strong> (box &rarr; boxes).</div>
          <div>&bull; <em>Нет:</em> Добавляем <strong>-S</strong> (play &rarr; plays, read &rarr; reads).</div>
        </li>
      </ol>
    </div>

    <!-- Interactive Exercises Section -->
    <div class="card">
      <h2>Интерактивная практика</h2>
      <p class="subtitle">Раскройте скобки, изменив глагол в соответствии с правилом:</p>

      <form @submit.prevent="checkAnswers">
        <div 
          v-for="(item, index) in exercises" 
          :key="item.id" 
          class="exercise-item"
        >
          <span>{{ index + 1 }}. {{ item.prefix }}</span>
          
          <input 
            v-model="userAnswers[item.id]" 
            type="text" 
            autocomplete="off"
          />
          
          <span>{{ item.suffix }}</span>

          <!-- Reactive Feedback -->
          <span 
            v-if="isSubmitted" 
            :class="['feedback', isCorrect(item.id) ? 'correct' : 'incorrect']"
          >
            {{ isCorrect(item.id) ? '✓ Верно!' : `✗ Ошибка (Правильно: ${item.answer})` }}
          </span>
        </div>

        <button type="submit" class="check-btn">Проверить ответы</button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'

// Exercise definitions
const exercises = [
  { id: 'q1', prefix: 'She (to read)', placeholder: 'read -> ?', suffix: 'a book every evening.', answer: 'reads' },
  { id: 'q2', prefix: 'He (to watch)', placeholder: 'watch -> ?', suffix: 'TV in the afternoon.', answer: 'watches' },
  { id: 'q3', prefix: 'The baby (to cry)', placeholder: 'cry -> ?', suffix: 'a lot at night.', answer: 'cries' },
  { id: 'q4', prefix: 'Alex (to fix)', placeholder: 'fix -> ?', suffix: 'his bicycle.', answer: 'fixes' },
  { id: 'q5', prefix: 'They (to study)', placeholder: 'study -> ?', suffix: 'English together.', answer: 'study' },
  { id: 'q6', prefix: 'It (to fly)', placeholder: 'fly -> ?', suffix: 'high in the sky.', answer: 'flies' }
]

// Reactive user inputs
const userAnswers = reactive({
  q1: '',
  q2: '',
  q3: '',
  q4: '',
  q5: '',
  q6: ''
})

const isSubmitted = ref(false)

// Logic to check individual answer correctness
const isCorrect = (id) => {
  const answer = exercises.find(ex => ex.id === id)?.answer
  return userAnswers[id].trim().toLowerCase() === answer
}

// Form submit handler
const checkAnswers = () => {
  isSubmitted.value = true
}
</script>

<style scoped>
@media screen and (width >= 900px) {
  .present-simple-container {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: auto auto;
    gap: 16px;
    max-width: 1024px;
  }
  .mnemonic-poem {
    min-width: 500px;
    grid-column: 1;
    grid-row: 1 / span 2;
  }

  .card {
    grid-column: 2;
    grid-row: 1;
    margin-top: 25px!important;
  }
  
  .card:last-child {
    grid-column: 2;
    grid-row: 2;
    margin-top: 0!important;
  }
}

.mnemonic-poem {
  max-width: 100%;
}
.present-simple-container {
  max-width: 900px;
  margin: 0 auto;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  color: #2c3e50;
}

.header {
  text-align: center;
  margin-bottom: 30px;
  font-family: ui-sans-serif, system-ui, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji";
}

.header span {
  color: #5b5b5b;
}


.header h1 {
  color: #1a365d;
  font-size: 2.2rem;
  margin-bottom: 10px;
}

.card {
  padding: 0 25px;
  margin-top: -30px;
}

.card:last-child {
  margin: 50px 0;
}

/* Algorithm Steps */
.step-list {
  list-style-type: none;
  counter-reset: step-counter;
  padding-left: 0;
}

.step-list li {
  position: relative;
  padding-left: 45px;
  margin-bottom: 15px;
  font-size: 1.05rem;
}

.step-list li::before {
  content: counter(step-counter);
  counter-increment: step-counter;
  position: absolute;
  left: 0;
  top: 0;
  width: 30px;
  height: 30px;
  background-color: #6d486e;
  color: white;
  border-radius: 50%;
  text-align: center;
  line-height: 30px;
  font-weight: bold;
}

/* Exercises */
.subtitle {
  margin-bottom: 20px;
}

.exercise-item {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 15px;
  font-size: 1.1rem;
  flex-wrap: wrap;
}

input[type="text"] {
  padding: 8px 12px;
  border: 2px solid #cbd5e1;
  border-radius: 8px;
  font-size: 1rem;
  width: 140px;
  outline: none;
  transition: border-color 0.2s;
}

input[type="text"]:focus {
  border-color: #4a90e2;
}

.check-btn {
  background-color: #6d486e;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 8px;
  font-size: 1rem;
  cursor: pointer;
  font-weight: bold;
  transition: background 0.2s;
  margin-top: 15px;
  width: 100%;
}

.check-btn:hover {
  background-color: #357abd;
}

.feedback {
  font-weight: bold;
  margin-left: 10px;
}

.feedback.correct { color: #2ecc71; }
.feedback.incorrect { color: #e74c3c; }
</style>
