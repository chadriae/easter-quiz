<script setup>
import { computed, ref } from 'vue'

const quizStarted = ref(false)

const answer = ref("")

const questions = ref([
  {
    question: "What is the capital of France?",
    answer: "Paris",
  },
  {
    question: "What is the largest country in the world?",
    answer: "Russia",
  },
  {
    question: "What is the currency of Japan?",
    answer: "Yen",
  }
])
const currentQuestionIndex = ref(0)
const selectedAnswerIndex = ref(null)
const showFeedback = ref(false)
const feedback = ref("")
const showNextQuestion = ref(false)
const showLink = ref(false)

const currentQuestion = computed(() => questions.value[currentQuestionIndex.value])

function startQuiz() {
  quizStarted.value = true
}

function selectAnswer() {
  showFeedback.value = true
  if (answer.value === currentQuestion.value.answer) {
    feedback.value = "CORRECT!"

    if (currentQuestionIndex.value === questions.value.length - 1) {
      showNextQuestion.value = false
      feedback.value = "Jullie kunnen nu echt de schat zoeken!"
      showLink.value = true
    } else {
      showNextQuestion.value = true
    }
  } else {
    showFeedback.value = true
    feedback.value = "Wrong answer."

    setTimeout(() => {
      feedback.value = ""
      showFeedback.value = false
    }, 2000)
  }
}

function nextQuestion() {
  answer.value = ""
  currentQuestionIndex.value++
  selectedAnswerIndex.value = null
  showFeedback.value = false
  showNextQuestion.value = false
}
</script>

<template>
  <div class="flex flex-col gap-4 text-center pt-6">
    <div v-if="!quizStarted">
      <button @click="startQuiz" class="px-16 py-4 bg-purple-600 hover:bg-yellow-500 rounded-2xl text-yellow-400 hover:text-purple-700 font-bold">Start!</button>
    </div>
    <div class="flex flex-col gap-4" v-if="quizStarted">
      <h2>{{ currentQuestion.question }}</h2>
      <input
          class="border border-gray-300 rounded-md p-2 max-w-lg self-center"
          @keyup.enter="selectAnswer()"
          v-model="answer" placeholder="Ik doe een gok"
      />
      <button
          class="px-16 py-4 bg-purple-600 hover:bg-yellow-500 rounded-2xl text-yellow-400 hover:text-purple-700 font-bold w-fit m-auto"
          @click="selectAnswer()"
      >
        Answer
      </button>
      <p v-if="showFeedback">{{ feedback }}</p>
      <button v-if="showNextQuestion" @click="nextQuestion" class="flex space-x-2 items-center m-auto text-green-400 border-b-2 border-dotted border-green-300">
        <span>Next Question</span>
        <span><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512" class="h-4" fill="#86efac"><path d="M438.6 278.6c12.5-12.5 12.5-32.8 0-45.3l-160-160c-12.5-12.5-32.8-12.5-45.3 0s-12.5 32.8 0 45.3L338.8 224 32 224c-17.7 0-32 14.3-32 32s14.3 32 32 32l306.7 0L233.4 393.4c-12.5 12.5-12.5 32.8 0 45.3s32.8 12.5 45.3 0l160-160z"/></svg></span>
      </button>
    </div>
    <div v-if="showLink">
      <div class="flex flex-col justify-center">
        <a href="https://maps.google.com" target="_blank"
           class="bg-blue-100 border border-blue-300 rounded-md py-2 px-8 self-center mt-8 flex flex-row min-w-lg text-sm group w-fit">
          <span class="mr-2">Zoek de schat</span>
          <span class="group-hover:translate-x-2 transition-transform duration-150">&rarr;</span>
        </a>

        <img class="m-auto w-64 h-auto mt-12" src="@/assets/fake_png.png"  alt="fake png"/>
      </div>
    </div>
  </div>
</template>
