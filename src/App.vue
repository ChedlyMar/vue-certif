<script setup>
import { ref } from 'vue'

const questionList = ref([
  {
    id: 1,
    question: 'what is our name',
    answer: 'name 1',
    options: ['name 1', 'name 2', 'name 3', 'name 4'],
  },
  {
    id: 2,
    question: 'what is our last name',
    answer: 'name 1',
    options: ['last name 1', 'last name 2', 'last name 3', 'last name 4'],
  },
])
const currentQuestionIndex = ref(0)
const currentQuestion = ref(questionList.value[currentQuestionIndex.value])
const selectedOption = ref('')
const isCorrect = ref(null)
const isAnswerd = ref(false)

const finalResult = ref(0)

const checkAnswer = () => {
  isAnswerd.value = true
  if (
    selectedOption.value ===
    questionList.value[currentQuestionIndex.value].answer
  ) {
    isCorrect.value = true
    finalResult.value++
  } else {
    isCorrect.value = false
  }
}

const nextQuestion = () => {
  isAnswerd.value = false
  isCorrect.value = null
  selectedOption.value = ''
  if (questionList.value.length > currentQuestionIndex.value) {
    currentQuestionIndex.value++
    currentQuestion.value = questionList.value[currentQuestionIndex.value]
  }
}
</script>

<template>
  <h1>app-certif</h1>
  <h2>{{ finalResult }}</h2>
  <div class="quesion-wraper">
    <h2>{{ currentQuestion.question }}</h2>
    <label
      v-for="option in currentQuestion.options"
      :key="`${currentQuestion.id}${option}`"
    >
      <input v-model="selectedOption" type="radio" :value="option" />
      {{ option }}
    </label>
    <div>
      <p v-if="isAnswerd">{{ isCorrect ? 'Correct' : 'Incorrect' }}</p>
      <button v-if="isAnswerd" @click="nextQuestion">Next</button>
      <button v-else @click="checkAnswer">Submit</button>
    </div>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
