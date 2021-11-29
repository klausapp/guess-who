<template>
  <div class="wrapper overlay">
    <img src="/klausmojis/dealwithklaus.png" />

    <h1>👋 Meowdy!</h1>
    <p>How well do you know your colleagues?</p>
    <p>...But how well do you know their feline spirit avatars — ✨ <em>klausmojis</em> ✨ ?</p>
    <p>
      I'll give you <strong>{{ count }}</strong> guesses, let's see what you score!
    </p>
    <p>You'll only have <strong>3 seconds</strong> for each of the last <strong>5</strong> guesses!</p>

    <button @click="emit('close')">Play!</button>
  </div>
</template>

<script setup lang="ts">
defineProps<{ count: number }>()
const emit = defineEmits(['close'])
</script>

<style scoped>
.wrapper {
  padding-top: 32px;
}

img {
  width: 140px;
  height: auto;
  margin-top: 32px;
}

.row {
  display: grid;
  align-items: center;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 16px;
  margin-top: 32px;
}
</style>
