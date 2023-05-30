<script setup lang="ts">
import { ref } from 'vue';
import Login from './Login.vue';
import GameView from './GameView.vue';



let showLogin = JSON.parse(localStorage.getItem('showLogin') || 'null');
let playerO = JSON.parse(localStorage.getItem('playerO') || 'null');
let playerX = JSON.parse(localStorage.getItem('playerX') || 'null');
localStorage.clear()
init();

function init(){
  if(showLogin == null){
    showLogin = ref(true);
  }
  if(playerO == null){
    playerO = ref({name: '', score: 0, symbol: 'O'});
  }
  if(playerX == null){
    playerX = ref({name: '', score: 0, symbol: 'X'});
    console.log(playerX.value)
  }
}


const addPlayerO = (newName: string) => {
  playerO.value.name = newName;
  localStorage.setItem('playerO', JSON.stringify(playerO));
  console.log(playerO.value.name)
}

const addPlayerX = (newName: string) => {
  playerX.value.name = newName;
  localStorage.setItem('playerX', JSON.stringify(playerX));
}

const startGame = () => {
  showLogin.value = false;
  localStorage.setItem('showLogin', JSON.stringify(showLogin))
}

const addPoint = (winner: string) => {
  if(winner == 'O'){
    playerO.value.score ++;
    //localStorage.setItem('playerO', JSON.stringify(playerO))
  } else if(winner == 'X'){
    playerX.value.score ++;
    //localStorage.setItem('playerX', JSON.stringify(playerX))
  }
}

const resetGame = () => {
  playerO.value = {name: '', score: 0, symbol: 'O'};
  playerX.value = {name: '', score: 0, symbol: 'X'};
  localStorage.clear();
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