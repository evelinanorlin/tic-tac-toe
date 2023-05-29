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

const handleWinner = (winner: string) => {
  if(winner == 'X'){
    console.log(winner)
  } else if(winner == 'O'){
    console.log(winner)
  } else{
    console.log(winner)
  }
}

</script>

<template>
  <h2>Welcome {{ props.playerO.name }} & {{ props.playerX.name }}</h2>
  <h1>Lets play Tic-Tac-Toe!</h1>
  <div>
    <GameBoard :playerO="playerO" :playerX="playerX" @winner="(winner: string) => handleWinner(winner)"/>
  </div>
  <div>
    <button>Start over</button>
    <button>Game statistics</button>
    <button>Reset game</button>
  </div>
</template>