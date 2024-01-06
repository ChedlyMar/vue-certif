<script setup>
import { ref } from 'vue'

const table = ref([
  {
    id: 1,
    value: '',
  },
  {
    id: 2,
    value: '',
  },
  {
    id: 3,
    value: '',
  },
  {
    id: 4,
    value: '',
  },
  {
    id: 5,
    value: '',
  },
  {
    id: 6,
    value: '',
  },
  {
    id: 7,
    value: '',
  },
  {
    id: 8,
    value: '',
  },
  {
    id: 9,
    value: '',
  },
])

const currentPlayer = ref('X')

const selectCell = (cell) => {
  if (!cell.value && !winner.value) {
    cell.value = currentPlayer.value
    checkWinner()
    currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X'
  }
}

const winner = ref('')
const checkWinner = () => {
  if (
    !winner.value &&
    ((table.value[0].value === table.value[1].value &&
      table.value[0].value === table.value[2].value &&
      table.value[0].value !== '') ||
      (table.value[3].value === table.value[4].value &&
        table.value[3].value === table.value[5].value &&
        table.value[3].value !== ''))
  ) {
    winner.value = currentPlayer.value
  }
}
const newGame = () => {
  winner.value = ''
  table.value = [
    {
      id: 1,
      value: '',
    },
    {
      id: 2,
      value: '',
    },
    {
      id: 3,
      value: '',
    },
    {
      id: 4,
      value: '',
    },
    {
      id: 5,
      value: '',
    },
    {
      id: 6,
      value: '',
    },
    {
      id: 7,
      value: '',
    },
    {
      id: 8,
      value: '',
    },
    {
      id: 9,
      value: '',
    },
  ]
  currentPlayer.value = 'X'
}
</script>

<template>
  <h1>app-certif</h1>
  <button @click="newGame">New game</button>
  <h2 v-if="winner">the winner is: {{ winner }}</h2>
  <div class="container">
    <div
      v-for="cell in table"
      :key="cell.id"
      class="box"
      @click="selectCell(cell)"
    >
      {{ cell.value }}
    </div>
  </div>
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
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 16px; /* Adjust the gap as needed */
}
.box {
  border: 1px solid #ddd;
  padding: 20px;
  text-align: center;
}
</style>
