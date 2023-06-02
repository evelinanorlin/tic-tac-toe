<script setup lang="ts">
  import { ref } from 'vue'; 

  let buttonDisabled = ref(JSON.parse(localStorage.getItem('buttonDisabled') ?? '"disabled"'));
  let playerOName = ref(JSON.parse(localStorage.getItem('playerOName') ?? 'null'));
  let playerXName = ref(JSON.parse(localStorage.getItem('playerXName') ?? 'null'));
  let checkStartArr = ref(JSON.parse(localStorage.getItem('checkStartArr') ?? '[]'));
  let playerO = ref('');
  let playerX = ref('');

  const emits = defineEmits<{
    (e: string, name: string): void
  }>()

  const startValues = () => {
    if(buttonDisabled.value == 'disabled'){
      buttonDisabled = ref(true);
    }
    if(!playerOName.value){
      playerOName = ref('');
    }
    if(!playerXName.value){
      playerXName = ref('');
    }
    if(!checkStartArr.value){
      checkStartArr =ref<string[]>([]);
    }
  }

  const sendPlayerO = () => {
    emits('playerO', playerO.value);
    checkStartArr.value = [...checkStartArr.value, '1']
    playerOName.value = playerO.value;
    localStorage.setItem('playerOName', JSON.stringify(playerOName.value));
    localStorage.setItem('checkStartArr', JSON.stringify(checkStartArr.value));

    if(checkStartArr.value.length >= 2){
      enableStartGame()
    }
  }

  const sendPlayerX = () => {
    emits('playerX', playerX.value);
    checkStartArr.value = [...checkStartArr.value, '1'];
    playerXName.value = playerX.value;

    localStorage.setItem('playerXName', JSON.stringify(playerXName.value));
    localStorage.setItem('checkStartArr', JSON.stringify(checkStartArr.value));

    if(checkStartArr.value.length >= 2){
    enableStartGame()
  }
  }

  const enableStartGame = () => {
    buttonDisabled.value = false;
    localStorage.setItem('buttonDisabled', JSON.stringify(buttonDisabled.value));
  }

  const startGame = () => {
    emits('start', 'start');
  }

  startValues();
</script>

<template>
  <h1>Welcome to Tic-Tac-Toe!</h1>
  <div class = "loginCont">
    <form>
      <input type="text" id="playerO" v-model="playerO" placeholder="Player O">
      <button @click.prevent="sendPlayerO">Add player O</button>
    </form>
    <form>
      <input type="text" id="playerX" v-model="playerX" placeholder="Player X">
      <button @click.prevent="sendPlayerX">Add player X</button>
    </form>
    <div>
      <p>Player O: {{ playerOName }}</p>
      <p>Player X: {{ playerXName }}</p>
    </div>
    <button :disabled="buttonDisabled" @click="startGame" class="startBtn">Start game!</button>
  </div>
</template>

<style>
  .loginCont{
    width: 100%;
    padding-left: 70px;
  }
</style>