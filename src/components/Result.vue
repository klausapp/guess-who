<template>
  <div class="wrapper overlay">
    <h1>Well-well...</h1>
    <p>{{ message }}</p>
    <div>
      <button @click="restart">Play again!</button>
    </div>

    <img :src="`/klausmojis/${image}`" />
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'
const props = defineProps<{ score: number }>()

const message = computed(() => {
  if (props.score < 5) return 'Oof... best to just go again and not mention this to anyone'
  if (props.score < 8) return 'Pretty good, but we know you have more in you!'
  if (props.score < 10) return 'Wow! Almost perfect!'
  return 'I yield... and crown you queen/king of Klausmojis!'
})

const image = computed(() => {
  if (props.score < 5) return 'facepaw.gif'
  if (props.score < 8) return 'stars.gif'
  if (props.score < 10) return 'hearts.gif'
  return 'uniklaus.png'
})

function restart() {
  location.reload()
}
</script>

<style scoped>
.wrapper {
  margin-top: 32px;
}

img {
  width: 200px;
  height: auto;
  margin-top: 32px;
}
</style>
