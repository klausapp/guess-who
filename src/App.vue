<template>
  <img alt="Klaus" class="logo" src="./assets/logo.svg" @click="reload" />
  <p>Score: {{ score }}</p>
  <Options :options="options" :image="image" @click="pick" />
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import Options from './components/Options.vue'
import answers from './answers'
import 'https://raw.githubusercontent.com/klausapp/design-tokens/master/dist/colors.css'

const used = ref<string[]>([])
const options = ref<string[]>([])
const correct = ref<string>()
const image = ref<number>()
const score = ref(0)

const unused = computed(() => {
  return answers.filter((id) => !used.value.includes(id))
})

function pick(id: string) {
  if (id === correct.value) score.value++

  console.info('correct?', id, correct.value, id === correct.value)
  prepRound()
}

function reload() {
  location.reload()
}

function getRandom (list = answers) {
  return list[list.length * Math.random() | 0]
}

function prepRound() {
  // TODO: Pick 3 random samples from unused
  // TODO: Pick one of those samples to be correct & look up its index from answers
  options.value = []
  const round = new Set<string>()

  while (round.size < 3) {
    round.add(getRandom())
  }

  console.info('round', round.values())
  options.value = [...round.values()]
  correct.value = getRandom(options.value)
  image.value = answers.findIndex((name) => name === correct.value)
  console.info('round prepped', correct.value)
}

function closeIntro() {
  prepRound()
}

prepRound()
</script>

<style>
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
</style>
