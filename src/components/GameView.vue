<script setup lang="ts">
import { stringifyExpression } from '@vue/compiler-core';
import GameBoard from './gameBoard.vue';

const props = defineProps<{playerO: {
  name: string,
  score: number, 
  symbol: string
}, playerX: {
  name: string,
  score: number,
  symbol: string
}}>()

</script>

<template>
  <h2>Welcome {{ props.playerO.name }} & {{ props.playerX.name }}</h2>
  <h1>Lets play Tic-Tac-Toe!</h1>
  <div>
    <GameBoard :playerO="playerO" :playerX="playerX"/>
  </div>
  <div>
    <button>Start over</button>
    <button>Game statistics</button>
    <button>Reset game</button>
  </div>
</template>