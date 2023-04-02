<script setup>
import { computed, reactive, ref } from 'vue'

let correct = reactive({answeredCorrectly: false, tries: 0})

let showButtonLink = reactive({showLink: false});

let showWrong = reactive({showText: false});

const startingQuestion = computed( () => {
  return Math.floor(Math.random() * quiz.length);
})

const quiz = [
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
]

const message = ref('')

function answerQuestion(number) {
  correct.tries++;

  if(message.value === quiz[number].answer) {
    correct.answeredCorrectly = true;

    setTimeout(() => {
      showButtonLink.showLink = true
    }, 1500)

    return correct.answeredCorrectly;
  }
  else {
    showWrong.showText = true;

    setTimeout(() => {
      showWrong.showText = false
    }, 1500)
  }
}

</script>

<template>
  <div class="flex flex-col gap-4 text-center">
    <div class="font-bold text-3xl">{{ quiz[startingQuestion].question }}</div>
    <input
        class="border border-gray-300 rounded-md p-2 max-w-lg self-center"
        @keyup.enter="answerQuestion(startingQuestion)"
        v-model="message" placeholder="Ik doe een gok"
    />
    <button
        class="bg-fuchsia-400 rounded-md py-2 px-8 w-fit self-center text-white font-bold"
        @click="answerQuestion(startingQuestion)"
    >
      Answer
    </button>

    <div v-if="!correct.answeredCorrectly && correct.tries >= 1 && showWrong.showText">Wrong! Try again.</div>

    <div class="flex flex-col gap-4" v-if="correct.answeredCorrectly">
      <span>Correct!</span>
      <div
          class="flex flex-col justify-center"
          v-show="showButtonLink.showLink">
        <a href="https://maps.google.com" target="_blank"
           class="bg-blue-100 border border-blue-300 rounded-md py-2 px-8 self-center mt-8 flex flex-row min-w-lg text-sm group w-fit">
          <span class="mr-2">Zoek de schat</span>
          <span class="group-hover:translate-x-2 transition-transform duration-150">&rarr;</span>
        </a>

        <img class="m-auto w-64 h-auto mt-12" src="@/assets/fake_png.png" />
      </div>
    </div>
  </div>
</template>
