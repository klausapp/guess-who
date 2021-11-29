<template>
  <div class="wrapper">
    <div class="row">
      <button v-for="opt in options" type="button" :key="opt" @click="$emit('click', opt)">
        {{ opt }}
      </button>
    </div>
    <div v-show="interval" class="timer">{{ timer }}</div>
    <img :src="`/klausmojis/${image}.png`" />
  </div>
</template>

<script setup lang="ts">
import { watch, Ref, ref } from 'vue'

const props = defineProps<{
  image: Ref<number>
  options: Ref<string[]>
  withTimer: boolean
}>()

const emit = defineEmits(['click'])
let timer = ref(3)
let interval = ref<number | undefined>(undefined)

const incrementTimer = () => {
  timer.value--
  if (timer.value <= 0) {
    resetTimer()
    emit('click', -1)
  }
}

const resetTimer = () => {
  clearInterval(interval.value)
  timer.value = 3
  interval.value = undefined
}

watch(
  () => props.image,
  () => {
    resetTimer()
    console.info('image watcher', props.withTimer)
    if (props.withTimer && !interval.value) interval.value = setInterval(incrementTimer, 1000)
  },
)
</script>

<style scoped>
.wrapper {
  position: relative;
  margin-top: 32px;
}

img {
  width: 80%;
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

.timer {
  position: absolute;
  bottom: 0;
  height: calc(100% - 48px);
  width: 100%;

  display: flex;
  align-items: center;
  justify-content: center;

  font-size: 128px;
  background-color: rgba(255, 255, 255, 0.2);
  font-weight: 600;
  color: white;
  text-shadow: 5px 5px var(--gray-80);
}
</style>
