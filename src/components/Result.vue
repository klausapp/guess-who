<template>
  <div class="wrapper overlay">
    <h1>Well-well...</h1>
    <h2>You scored: {{ score }}</h2>
    <p>{{ message }}</p>
    <div>
      <button @click="restart">Play again!</button>
    </div>

    <img :src="`/klausmojis/${image}`" :class="image === 'uniklaus.png' && 'shake'" />
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'
const props = defineProps<{ score: number }>()

const message = computed(() => {
  if (props.score === 15) return 'I yield... and crown you queen/king of Klausmojis!'
  if (props.score > 12) return 'Wow! Almost perfect!'
  if (props.score > 10) return 'Pretty good, but we know you have more in you!'
  if (props.score > 8) return `If you spend a couple more hours on this, you'll really get somewhere`
  return 'Oof... best to just go again and not mention this to anyone'
})

const image = computed(() => {
  if (props.score === 15) return 'uniklaus.png'
  if (props.score > 12) return 'hearts.gif'
  if (props.score > 8) return 'stars.gif'
  return 'facepaw.gif'
})

function restart() {
  location.reload()
}
</script>

<style scoped>
.wrapper {
  padding-top: 32px;
}

img {
  width: 200px;
  height: auto;
  margin-top: 32px;
}

.shake {
  animation: shake 0.2s infinite ease-in-out both;
  transform: translate3d(0, 0, 0);
  backface-visibility: hidden;
  perspective: 1000px;
}

@keyframes shake {
  20%,
  60% {
    transform: translate(-10px, -5px);
  }

  40%,
  80% {
    transform: translate(10px, 5px);
  }
}
</style>
