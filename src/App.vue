<script setup>
import { ref, computed } from '@vue/reactivity';
const player = ref('x')
const board = ref([
  ['','',''],
  ['','',''],
  ['','',''],
])

const CalculateWinner = (squares) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}

const winner = computed(() => CalculateWinner(board.value.flat()))

const MakeMove = (x, y) => {
  if(winner.value) return

  if(board.value[x][y] !== '') return

  board.value[x][y] = player.value

  player.value = player.value === 'x' ? 'o' : 'x'
}

const ResetGame = () => {
  board.value = ref([
    ['','',''],
    ['','',''],
    ['','',''],
  ])
  player.value = ref('x')
}

</script>

<template>
  <main class="pt-8 text-center bg-slate-800 min-h-screen text-white">
    <h1 class="mb-8 text-3x1 font-bold uppercase">Tic Toc Toe</h1>

    <h3 class="tetx-xl mb-4">Player {{player}}'s turn </h3>

    <div class="flex flex-col items-center mb-8">
      <div
        v-for="(row, x) in board" 
        :key="x"
        class="flex"
      >
        <div 
          v-for="(cell, y) in row" 
          key="y"
          @click="MakeMove(x, y)"
          :class="`border border-white w-20 h-20 hover:bg-gray-700 flex items-center justify-center 
          material-icons-outlined text-4x1 cursor-pointer 
          ${cell === 'x' ? 'text-pink-500' : 'text-blue-500'}`"
        >
          {{cell === 'x' ? 'close' : cell === 'o' ?  'circle' : ''}}
        </div>
      </div>
    </div>

    <h2 v-if="winner" class="text-6x1 font-bold mb-8">
      Player '{{winner}}' wins
    </h2>

    <button @click="ResetGame" class="px-4 py-2 bg-pink-500 roundedd uppercase font-bold hover:bg-pink-600 duration-300">
      Reset Game
    </button>
  </main>
</template>

<style scoped></style>
