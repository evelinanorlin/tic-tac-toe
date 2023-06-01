<script setup lang="ts">
import GameBoard from './GameBoard.vue';

const props = defineProps<{
playerO: {name: string, score: number, symbol: string, class: string }, 
playerX: {name: string, score: number, symbol: string, class: string
}}>()

const emits = defineEmits<{
  (e: string, winner: string): void,
}>();

const handleWinner = (winner: string) => {
  if(winner == 'X'){
    emits('addPoint', 'X');
  } else if(winner == 'O'){
    emits('addPoint', 'O');
  }
}

const resetGame = () => {
  emits('reset', '');
}
</script>

<template>
  <h1>Lets play Tic-Tac-Toe!</h1>
  <div class="gameFlex">
    <div>
      <GameBoard :playerO="playerO" :playerX="playerX" @winner="(winner: string) => handleWinner(winner)"/>
    </div>
  <div class="gameStats">
    <h2>Game stats</h2>
    <p>{{ props.playerO.name }}: {{ props.playerO.score }} points</p>
    <p>{{ props.playerX.name }}: {{ props.playerX.score }} points</p>
    <button @click="resetGame" class="resetBtn">Reset game</button>
  </div>
</div>
</template>

<style>
  .gameFlex{
    display: flex;
    justify-content: space-between;
  }

  .gameStats{
    border: 1px solid black;
    height: 200px;
    padding: 0 20px 0 20px;
    margin-top: 70px;
    background: #dadada;
  }

  .resetBtn{
    margin-top: 5px;
  }
</style>