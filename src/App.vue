<script setup>
import { ref } from 'vue'

const todoList = ref([
  { id: 1, task: 'tsak 1', isComplete: false },
  { id: 2, task: 'tsak 2', isComplete: true },
  { id: 3, task: 'tsak 3', isComplete: false },
])

const newTask = ref('')

const addTask = () => {
  todoList.value.push({
    id: Math.floor(Math.random() * 9999),
    task: newTask.value,
    isComplete: false,
  })
  newTask.value = ''
}

const deleteTask = (taskId) => {
  todoList.value = todoList.value.filter((todo) => todo.id !== taskId)
}

const isShow = ref(true)
const toggleList = () => {
  isShow.value = !isShow.value
}
</script>

<template>
  <h1>app-certif</h1>
  <div>
    <label for="">Task</label>
    <input v-model="newTask" type="text" />
    <button @click="addTask">Add task</button>
  </div>
  <button @click="toggleList">show List !</button>
  <!-- <Transition name="task" appear> -->
  <TransitionGroup tag="ul" name="element" appear>
    <li v-for="todo in todoList" :key="todo.id">
      <span>{{ todo.task }}</span>
      <label for="">
        Complete
        <input v-model="todo.isComplete" type="checkbox" />
      </label>
      <button @click="deleteTask(todo.id)">Delete</button>
    </li>
  </TransitionGroup>
  <!-- </Transition> -->
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

.task-enter-from {
  opacity: 0;
}
.task-enter-to {
  opacity: 1;
}
.task-enter-active {
  transition: all 2s ease-in;
}

.task-leave-from {
  opacity: 1;
}
.task-leave-to {
  transform: translate(50px);
  opacity: 0;
}
.task-leave-active {
  transition: all 2s;
}

.element-enter-from {
  transform: translateX(50px);
}
.element-enter-to {
}

.element-leave-from {
  opacity: 1;
}
.element-leave-to {
  transform: translateX(50px);
  opacity: 0;
}
.element-move,
.element-enter-active,
.element-leave-active {
  transition: all 2s ease;
}
.element-leave-active {
  position: absolute;
}
</style>
