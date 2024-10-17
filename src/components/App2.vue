<script>
export default {
  name: 'AppTwo', // Keep this as is
}
</script>

<script setup>
import { ref, onBeforeMount } from 'vue'

const userName = 'John Doe' // Renamed from 'name' to 'userName'
let status = ref('active')
const tasks = ref(['TASK 1', 'TASK 2', 'TASK 3'])
const link = 'https://github.com/JohnDoe' // Fixed the URL by removing the space
const toggleStatus = () => {
  status.value =
    status.value === 'active'
      ? 'inactive'
      : status.value === 'inactive'
        ? 'pending'
        : status.value === 'pending'
          ? 'active'
          : 'active'
}

const getTasks = async () => {
  try {
    const response = await fetch(
      'https://jsonplaceholder.typicode.com/todos?limit=10',
    )
    const data = await response.json()
    tasks.value = data.map(task => task.title)
  } catch (e) {
    console.log(e)
  }
}
const removeItem = index => {
  tasks.value.splice(index, 1)
}
onBeforeMount(async () => {
  await getTasks()
})
</script>
<style>
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>
<template>
  <div>Name: {{ userName }}</div>
  <!-- Updated to use 'userName' -->
  <h1>Status : {{ status }}</h1>
  <ul>
    <transition-group name="list">
      <li v-for="(task, index) in tasks" :key="task">
        {{ task }}
        <span @click="removeItem(index)" style="color: white">X</span>
      </li>
    </transition-group>
  </ul>
  <button @click="tasks.push('TASK 4')">Push task</button>
  <a :href="link" target="_blank">GO to link</a>
  <button @click="toggleStatus">Change user Status</button>
</template>
