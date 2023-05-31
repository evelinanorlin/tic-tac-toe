<script setup lang="ts">
import GameBoard from './GameBoard.vue';

const props = defineProps<{playerO: {
  name: string,
  score: number, 
  symbol: string
}, playerX: {
  name: string,
  score: number,
  symbol: string
}}>()

const emits = defineEmits<{
  (e: string, winner: string): void,
}>();

const handleWinner = (winner: string) => {
  if(winner == 'X'){
    emits('addPoint', 'X');
  } else if(winner == 'O'){
    emits('addPoint', 'O');
  } else{
    console.log(winner)
  }
}

const resetGame = () => {
  emits('reset', '');
}

</script>

<template>
  <h2>Welcome {{ props.playerO.name }} & {{ props.playerX.name }}</h2>
  <h1>Lets play Tic-Tac-Toe!</h1>
  <div>
    <GameBoard :playerO="playerO" :playerX="playerX" @winner="(winner: string) => handleWinner(winner)"/>
  </div>
  <div>
    <h2>Game stats</h2>
    <p>{{ props.playerO.name }}: {{ props.playerO.score }} points</p>
    <p>{{ props.playerX.name }}: {{ props.playerX.score }} points</p>
    <button @click="resetGame">Reset game</button>
  </div>
</template>