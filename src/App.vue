<template>
  <img alt="Klaus" class="logo" src="./assets/logo.svg" @click="reload" />

  <button class="info-btn" @click="showIntro">
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="24"
      height="24"
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      stroke-width="2"
      stroke-linecap="round"
      stroke-linejoin="round"
      class="feather feather-info"
    >
      <circle cx="12" cy="12" r="10"></circle>
      <line x1="12" y1="16" x2="12" y2="12"></line>
      <line x1="12" y1="8" x2="12.01" y2="8"></line>
    </svg>
  </button>

  <p>Score: {{ score }}</p>
  <p>Guesses left: {{ GAME_LENGTH - guessCount + 1 }}</p>

  <Options :options="options" :image="image" @click="pick" />
  <Instructions v-show="showInstructions" @close="closeIntro" />
  <Result v-show="showResults" :score="score" />
</template>

<script setup lang="ts">
import { ref, computed, ButtonHTMLAttributes } from 'vue'
import { useHead } from '@vueuse/head'

import Options from './components/Options.vue'
import Instructions from './components/Instructions.vue'
import Result from './components/Result.vue'
import answers from './answers'

const GAME_LENGTH = 10
const INSTRUCTIONS_DISMISSED = localStorage.hideInstructions

const used = ref<string[]>([])
const options = ref<string[]>([])
const correct = ref<string>()
const image = ref<number>(0)
const score = ref(0)
const guessCount = ref(1)

const showInstructions = ref(!INSTRUCTIONS_DISMISSED)
const showResults = ref(false)

const unused = computed(() => {
  return answers.filter((id) => !used.value.includes(id))
})

function pick(answer: string) {
  if (answer === correct.value) score.value++
  used.value.push(answer)
  guessCount.value++

  if (guessCount.value > GAME_LENGTH) showResults.value = true
  else prepRound()
  ;(document.activeElement as any).blur()
}

function reload() {
  location.reload()
}

function getRandom(list: string[]) {
  return list[(list.length * Math.random()) | 0]
}

function prepRound() {
  options.value = []
  const round = new Set<string>()

  while (round.size < 3) round.add(getRandom(unused.value))

  options.value = [...round.values()]
  correct.value = getRandom(options.value)
  image.value = answers.findIndex((name) => name === correct.value) + 1 || 0
}

function showIntro() {
  showInstructions.value = true
  localStorage.hideInstructions = false
}

function closeIntro() {
  showInstructions.value = false
  localStorage.hideInstructions = true
  prepRound()
}

function endGame() {
  showResults.value = true
}

useHead({
  title: 'Guess who!?',
  meta: [
    {
      name: 'description',
      content: `Guess the klausmoji`,
    },
    {
      name: 'og:image',
      content: `/preview.png`,
    },
  ],
})

prepRound()
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&display=swap');

#app {
  font-family: 'Inter', -apple-system, system-ui, BlinkMacSystemFont, 'Segoe UI', Roboto, Ubuntu;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  max-width: 40rem;
  width: 96vw;
  margin: 60px auto;
}

.logo {
  width: 48px;
  height: auto;
  cursor: pointer;
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: white;
}

button {
  background: none;
  border: none;
  padding: 8px 12px;
  text-transform: uppercase;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 500;
  justify-self: center;
  background-color: var(--brand-10);
}
button:hover {
  background-color: var(--brand-50);
  color: white;
}

.info-btn {
  position: absolute;
  top: 32px;
  left: 32px;

  display: flex;
  align-items: center;
  justify-content: center;

  width: 32px;
  height: 32px;
  border-radius: 50%;
  padding: 0;
}
.info-btn svg {
  height: 16px;
  width: 16px;
}
</style>
