<script setup lang="ts">
import { ref } from 'vue'

const count = ref(0)

function increment() {
  count.value++
}
</script>

<template>
  <h1>Hello</h1>
  <p>You're count is: {{ count }}</p>
  <button @click="increment">Add to count</button>
</template>

<!-- <style>
.button {
  max-width: 100px;
}
</style> -->
