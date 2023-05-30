<script setup lang="ts">
import { ref } from 'vue';
import Login from './Login.vue';
import GameView from './GameView.vue';

let showLogin = ref(true);

let playerO = ref({name: '', score: 0, symbol: 'O'})
let playerX = ref({name: '', score: 0, symbol: 'X'})

const addPlayerO = (newName: string) => {
  playerO.value.name = newName;
}

const addPlayerX = (newName: string) => {
  playerX.value.name = newName;
}

const startGame = () => {
  showLogin.value = false;
}

const addPoint = (winner: string) => {
  if(winner == 'O'){
    playerO.value.score ++;
  } else if(winner == 'X'){
    playerX.value.score ++;
  }
}

const resetGame = () => {
  playerO.value = {name: '', score: 0, symbol: 'O'};
  playerX.value = {name: '', score: 0, symbol: 'X'};
  showLogin.value = true;
}
</script>

<template>
  <div v-if="showLogin">
    <Login @playerO="addPlayerO" @playerX="addPlayerX" @start="startGame"/>
  </div>

  <div v-else>
    <GameView :playerO="playerO" :playerX="playerX" @addPoint="addPoint" @reset="resetGame"/>
  </div>
</template>