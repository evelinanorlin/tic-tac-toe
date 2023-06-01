<script setup lang="ts">
import { ref } from 'vue';
import Login from './Login.vue';
import GameView from './GameView.vue';

let showLogin = ref(JSON.parse(localStorage.getItem('showLogin') ?? '"start"'));
let playerO = ref(JSON.parse(localStorage.getItem('playerO') ?? 'null'));
let playerX = ref(JSON.parse(localStorage.getItem('playerX') ?? 'null'));

function createVariables(){
  if(showLogin.value == 'start'){
    showLogin = ref(true);
  }
  if(!playerO.value){
    playerO = ref({name: '', score: 0, symbol: 'O'});
  }
  if(!playerX.value){
    playerX = ref({name: '', score: 0, symbol: 'X'});
    console.log(playerX.value)
  }
}

const addPlayerO = (newName: string) => {
  playerO.value.name = newName;
  localStorage.setItem('playerO', JSON.stringify(playerO.value));
  console.log(playerO.value.name)
}

const addPlayerX = (newName: string) => {
  playerX.value.name = newName;
  localStorage.setItem('playerX', JSON.stringify(playerX.value));
}

const startGame = () => {
  showLogin.value = false;
  localStorage.setItem('showLogin', JSON.stringify(showLogin.value))
}

const addPoint = (winner: string) => {
  if(winner == 'O'){
    playerO.value.score ++;
    localStorage.setItem('playerO', JSON.stringify(playerO.value))
  } else if(winner == 'X'){
    playerX.value.score ++;
    localStorage.setItem('playerX', JSON.stringify(playerX.value))
  }
}

const resetGame = () => {
  playerO.value = {name: '', score: 0, symbol: 'O'};
  playerX.value = {name: '', score: 0, symbol: 'X'};
  showLogin.value = true;
  localStorage.clear();
}

createVariables();
</script>

<template>
  <div v-if="showLogin">
    <Login @playerO="addPlayerO" @playerX="addPlayerX" @start="startGame"/>
  </div>

  <div v-else>
    <GameView :playerO="playerO" :playerX="playerX" @addPoint="addPoint" @reset="resetGame"/>
  </div>
</template>

<style>
  body{
    width: 500px;
    margin: 0 auto;
    font-family: 'Montserrat', sans-serif;
  }

  body, input, button{
    font-family: 'Montserrat', sans-serif;
  }

  h1, h2, h3{
    font-family: 'Poiret One', cursive;
  }
</style>