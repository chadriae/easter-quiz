<script setup>
import { computed, reactive, ref } from 'vue'

let correct = reactive({answeredCorrectly: false, tries: 0})

let showButtonLink = reactive({showLink: false});

const startingQuestion = computed( () => {
  return Math.floor(Math.random() * quiz.length);
})

const quiz = reactive([
{
    question: 'How much 2 + 2?',
    answer: '4'
  },
    {
    question: 'What is the capital of Belgium?',
    answer: 'Brussels'
  },
    {
    question: 'What is the name of the first president of the United States?',
    answer: 'Washington'
  },
    {
    question: 'Who is the author of the book "The Lord of the Rings"?',
    answer: 'Tolkien'
  },
  {
    question: 'Vul aan: Starring ...',
    answer: 'Jane'
  },
  {
    question: 'Mijn leeftijd is ...',
    answer: '34'
  }
])

const message = ref('')

function answerQuestion(number) {
  correct.tries++;

  if(message.value === quiz[number].answer) {
    correct.answeredCorrectly = true;

    setTimeout(() => {
      showButtonLink.showLink = true
    }, 2000)

    return correct.answeredCorrectly;
  }
  else {
    correct.answeredCorrectly = false;
    return correct.answeredCorrectly;
  }
}

</script>

<template>
  <div class="flex flex-col gap-4 text-center">
    <div class="font-bold text-3xl">{{ quiz[startingQuestion].question }}</div>
    <input
        class="border border-gray-300 rounded-md p-2"
        @keyup.enter="answerQuestion(startingQuestion)"
        v-model="message" placeholder="edit me"
    />
    <button
        class="bg-green-100 border border-green-300 rounded-md py-2 px-8 w-fit self-center"
        @click="answerQuestion(startingQuestion)"
    >
      Answer
    </button>
    <div v-if="!correct.answeredCorrectly && correct.tries >= 1">Wrong! Try again.</div>
    <div class="flex flex-col gap-4" v-if="correct.answeredCorrectly">
      <span>Correct!</span>
      <div v-show="showButtonLink.showLink">
        <a href="https://maps.google.com" target="_blank"
           class="bg-blue-100 border border-blue-300 rounded-md py-2 px-8 w-fit self-center mt-8">
          Klik hier om naar de volgende locatie te gaan
        </a>
      </div>
    </div>
  </div>
</template>
