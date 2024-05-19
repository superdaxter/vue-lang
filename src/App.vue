<script setup lang="ts">
import { ref, reactive, computed, onMounted, watch } from 'vue'
import WelcomeItem from './components/WelcomeItem.vue'

const msg1 = ref('Hello World!')
const msg2 = ref('I am the President of the United States!')
const msg3 = ref('')
const hideCompleted = ref(false)
const swapStyle = ref(false)
const pElementRef = ref()

onMounted(() => {
  ;(pElementRef.value.style.color = 'blue'), (pElementRef.value.textContent = 'Hey Brother!')
})

let id = 0
const newItem = ref('')
const items = ref([
  { id: id++, text: 'Abcs Cuh', done: false },
  { id: id++, text: '123s Cuh', done: false },
  { id: id++, text: 'YEAT bruh', done: false }
])

function myLog() {
  console.log(items.value, document.documentElement.classList)
}

const filteredItems = computed(() => {
  return hideCompleted.value ? items.value.filter((e) => !e.done) : items.value
})

const leData = reactive({
  count: 0,
  check: true
})

// Learn how to use Watchers
const num = ref(1)
const itemData = ref(null)

watch(num, () => {
  console.log(`new number is:${num.value}`)
})

async function fetchData() {
  itemData.value = null
  const res = await fetch(`https://jsonplaceholder.typicode.com/todos/${num.value}`)
  itemData.value = await res.json()
}

fetchData()
watch(num, fetchData)

function increment() {
  leData.count++
}

function addItem() {
  if (newItem.value) {
    items.value.push({ id: id++, text: newItem.value, done: false })
    newItem.value = ''
  }
}

function removeItem(item: { id: number }) {
  items.value = items.value.filter((e) => e.id !== item.id)
}

function swap() {
  leData.check = !leData.check
}
</script>

<template>
  <div class="leBlock">
    <h1 v-if="leData.check" class="title" :class="{ done: swapStyle }">{{ msg1 }}</h1>
    <h1 v-else :class="{ done: swapStyle }">{{ msg2 }}</h1>
    <a class="block">Your count is: {{ leData.count }}</a>
    <button @click="increment" class="block">Click me!</button>
    <button @click="swap" class="block">Toggle text</button>
    <button @click="swapStyle = !swapStyle" class="block">Toggle le Style</button>
    <h2>Lets bind some form stuff:</h2>
    <form @submit.prevent="addItem">
      <input v-model="newItem" placeholder="Enter Text Cuh" />
      <button>Add me!</button>
    </form>
    <p>Your text: {{ newItem }}</p>
    <h2>My List</h2>
    <ul>
      <li v-for="item in filteredItems" :key="item.id">
        <input type="checkbox" v-model="item.done" />
        <span :class="{ done: item.done }">{{ item.text }}</span>
        <button @click="removeItem(item)">X</button>
      </li>
    </ul>
    <button @click="hideCompleted = !hideCompleted">
      {{ !hideCompleted ? 'Hide Completed' : 'Unhide Completed' }}
    </button>
    <!-- Trying out Template refs -->
    <p ref="pElementRef">hello</p>
    <!-- watcher -->
    <p>{{ num }}</p>
    <button @click="num++">num ++</button>
    <p v-if="!itemData">Loading...</p>
    <pre v-else>{{ itemData }}</pre>
  </div>
  <div class="leBlock2">
    <WelcomeItem :theItems="filteredItems" @response="(e) => (msg3 = e)">
      {{ msg1 }}
    </WelcomeItem>
    <button @click="myLog">Log le Props</button>
    <p>
      Le Childs Msg is: <span style="font-weight: 700">{{ msg3 }}</span>
    </p>
  </div>
</template>

<style>
.block {
  display: block;
}

.leBlock {
  float: left;
  width: 45%;
}

.leBlock2 {
  float: right;
  width: 45%;
}

.title {
  color: red;
}

* {
  margin: 10px;
}

.done {
  text-decoration: line-through;
}
</style>
