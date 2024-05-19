<script setup lang="ts">
import JSConfetti from 'js-confetti'
const confetti = new JSConfetti()
function showConfetti() {
  confetti.addConfetti()
}

showConfetti()

defineProps({
  theItems: Array
})

const emit = defineEmits(['response'])
emit('response', 'Hello from Vice President of the USA')
</script>

<template>
  <h2>Yooo THIS IS WELCOME ITEM!</h2>
  <ul v-for="item in theItems" :key="item.id">
    <li>{{ item.text }}</li>
  </ul>
  <div>
    <h2>Youre slot content is below:</h2>
    <slot> Fallback Content </slot>
    <button @click="showConfetti">SHOW CONFETTI CUH</button>
    <button @click="showConfetti">SWITCH MODEEE</button>
  </div>
</template>
