<template>
  <main class="pt-8 text-center">
    <h1 class="mb-8 text-3xl font-bold">Boter Kaas en Eieren</h1>

    <h3 class="text-xl mb-4">Speler {{ player }} is aan de beurt</h3>

    <div class="flex flex-col items-center mb-8">
      <div
        v-for="(row, x) in board"
        :key="x"
        class="flex">

        <div v-for="(cell, y) in row" 
        :key="y"
        @click="MakeMove(x, y)"
        :class="`border border-black w-20 h-20 hover:bg-gray-700 flex items-center justify-center material-icons-outlined text-4xl cursor-pointer ${ cell === 'X' ? 'text-red-500' : 'text-green-500'}`">
        {{ cell === 'X' ? 'X' : cell === 'O' ? 'O' : '' }}
      </div>
      </div>
    </div>

    <h2 v-if="winner" class="text-6x1 font-bold mb-8">Speler '{{ winner }}' wint!</h2>
    <h2 v-else-if="tie" class="text-3xl font-bold mb-8">Gelijkspel!</h2>

    <button @click="ResetGame" class="px-4 py-2 bg-pink-500 rounded uppercase font-bold hover:bg-pink-600 duration-300">Reset Spel</button>
  </main>
</template>

<script setup>
import {ref, computed } from 'vue'

const player = ref('X');
const board = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', ''],
])

const calculateWinner = (squares)=> {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}

const winner = computed(() => calculateWinner(board.value.flat()))
const tie = computed(() => !winner.value && board.value.flat().every(cell => cell !== ''))

const MakeMove = (x, y) => {
  if(winner.value) return

  if (board.value[x][y] !== '') return

  board.value[x][y] = player.value

  player.value = player.value === 'X' ? 'O' : 'X'
}

const ResetGame = () => {
  board.value = [
  ['', '', ''],
  ['', '', ''],
  ['', '', ''],
]
  player.value = 'X'
}

</script>



<style>

</style>
