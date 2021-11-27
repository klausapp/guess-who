<template>
  <div class="wrapper">
    <div class="row">
      <button v-for="opt in options" type="button" :key="opt" @click="$emit('click', opt)">
        {{ opt }}
      </button>
    </div>

    <img :src="`/klausmojis/${image}.png`" />
  </div>
</template>

<script setup lang="ts">
defineProps<{
  image: number
  options: string[]
}>()

const emit = defineEmits(['click'])
</script>

<style scoped>
.wrapper {
  margin-top: 32px;
}

img {
  width: 100%;
  height: auto;
}

.row {
  display: grid;
  align-items: center;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 16px;
  margin-top: 32px;
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
</style>
