<!-- https://it.vuejs.org/tutorial/#step-2
 -->
<script setup lang="ts">

import { computed, onMounted, reactive, ref, watch } from 'vue'
import ChildComp from "./ChildComp.vue"

// 2
const message = ref('Hello World!')
const counter = reactive({ count: 0 })

// 3
const titleClass = ref('title')

// 4
const count = ref(0)
function increment() {
  count.value++
}
function increment2() {
  counter.count++
}

// 5
const text = ref('')
function onInput(e: Event) {
  console.log("Tipo dell'evento:", typeof e) // object
  console.log("Evento:", e) // object
  text.value = (e.target as HTMLInputElement).value + "PIPPO"
}

// 6
const awesome = ref(true)
function toggle() {
  awesome.value = !awesome.value
}

// 7
type todoType = {
  id: number;
  text: string;
};

let id = 0

const newTodo = ref('')
const todos = ref([
  { id: id++, text: 'Learn HTML' },
  { id: id++, text: 'Learn JavaScript' },
  { id: id++, text: 'Learn Vue' }
])

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value })
  newTodo.value = ''
}

function removeTodo(todo: todoType) {
  // todos.value = todos.value.filter((t) => t.id !== todo.id) // non necessario specificare l'id
  todos.value = todos.value.filter((t) => t !== todo)
}

// 8

const newTodo2 = ref('')
const hideCompleted = ref(false)

const todos2 = ref([
  { id: id++, text: 'Learn HTML', done: true },
  { id: id++, text: 'Learn JavaScript', done: true },
  { id: id++, text: 'Learn Vue', done: false }
])

const filteredTodos = computed(() => {
  return hideCompleted.value
    ? todos2.value.filter((t) => !t.done)
    : todos2.value
  // return todos2.value.filter((t) => !t.done || hideCompleted.value)
})

function addTodo2() {
  todos2.value.push({ id: id++, text: newTodo2.value, done: false })
  newTodo2.value = ''
}

function removeTodo2(todo: todoType) {
  todos2.value = todos2.value.filter((t) => t !== todo)
}

// 9
const pElementRef = ref<HTMLElement | null>(null)

onMounted(() => {
  pElementRef.value && (pElementRef.value.textContent = 'mounted!')
  // if (pElementRef.value !== null) {
  //   pElementRef.value.textContent = 'mounted!';
  // }
})

// 10
const todoId = ref(1)
const todoData = ref(null)

async function fetchData() {
  todoData.value = null
  const res = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )
  await sleep(2000)
  todoData.value = await res.json()
}

// https://www.geeksforgeeks.org/how-to-implement-sleep-function-in-typescript/
async function sleep(ms: number): Promise<void> {
  return new Promise(
    (resolve) => setTimeout(resolve, ms));
}

fetchData()

watch(todoId, fetchData)

// 12
const greeting = ref('Ciao dal padre')

// 13
const childMsg = ref('Ancora nessun messaggio dal figlio')

// 14
const msg = ref('dal padre')


</script>

<template>
  <!-- 2 -->
  <h1>{{ message }}</h1>
  <p>Count is: {{ counter.count }}</p>

  <!-- 3 -->
  <h1 :class="titleClass">Rendimi rosso</h1>

  <!-- 4 -->
  <p>Count is: {{ count }}</p>
  <button @click="increment">conteggio: {{ count }}</button> <br><br>
  <button @click="increment2">conteggio (counter.count): {{ counter.count }}</button>

  <!-- 5 -->
  <br><br>
  <!-- senza onInput -->
  <input v-model="text" placeholder="Scrivi qui">
  <p>{{ text }}</p>
  <input :value="text" @input="onInput" placeholder="Type here">

  <!-- 6 -->
  <br><br>
  <button @click="toggle">toggle</button>
  <h1 v-if="awesome">Vue è fantastico!</h1>
  <h1 v-else>Oh no 😢</h1>

  <!-- 7 -->
  <h3>7</h3>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo">
    <button>Aggiungi un todo</button>
  </form>
  <ul>
    <li v-for="todo in todos" :key="todo.id">
      {{ todo.text }}
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>

  <!-- 8 -->
  <h3>8</h3>
  <form @submit.prevent="addTodo2">
    <input v-model="newTodo2">
    <button>Aggiungi Todo</button>
  </form>
  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done">
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
      <button @click="removeTodo2(todo)">X</button>
    </li>
  </ul>
  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Mostra tutti' : 'Nascondi completati' }}
  </button>

  <!-- 9 -->
  <h2>9</h2>
  <p ref="pElementRef">ciao</p>

  <!-- 10 -->
  <h2>10</h2>
  <p>Todo id: {{ todoId }}</p>
  <button @click="todoId++">Fetch del todo successivo</button>
  <p v-if="!todoData">Caricamento in corso...</p>
  <pre v-else>{{ todoData }}</pre>

  <!-- 11 -->
  <h2>11</h2>
  <ChildComp />

  <!-- 12 -->
  <h2>12</h2>
  <ChildComp :msg="greeting" />

  <!-- 13 -->
  <h2>13</h2>
  <ChildComp @response="(msg) => childMsg = msg" />
  <p>{{ childMsg }}</p>

  <!-- 14 -->
  <h1>14</h1>
  <ChildComp>Messaggio (tramite slot): {{ msg }}</ChildComp>





</template>

<style>
.title {
  color: red;
}

/* 8 */
.done {
  text-decoration: line-through;
}
</style>

<style scoped lang="scss"></style>