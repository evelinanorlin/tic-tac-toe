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
  }
}

const addPlayerO = (newName: string) => {
  playerO.value.name = newName;
  localStorage.setItem('playerO', JSON.stringify(playerO.value));
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
    margin: 100px auto;
    font-family: 'Montserrat', sans-serif;
  }

  body, input, button{
    font-family: 'Montserrat', sans-serif;
  }

  h1, h2, h3{
    font-family: 'Poiret One', cursive;
  }

  input{
    height: 25px;
    padding: 0;
    margin: 0;
    width: 180px;
    margin-right: 20px;
    margin-bottom: 20px;
  }

  h1{
    text-align: center;
    margin-bottom: 40px;
  }

  button{
    width: 160px;
    height: 30px;
    background: #B7D1BE;
    border: 1px solid black;
    border-radius: 30px;
    cursor: pointer;
    transition: all .3s;
    font-size: 1rem;
  }

  button:disabled{
    background: #d3d3d3;
    color: #5a5a5a;
    border: 1px solid #5a5a5a;
    cursor: default;
  }

  button:hover:enabled{
    background: #DB7248;
  }
</style>