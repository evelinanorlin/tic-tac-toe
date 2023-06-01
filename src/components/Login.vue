<script setup lang="ts">
  import { ref } from 'vue'; 

  let playerO = ref('');
  let playerX = ref('');
  let playerOName = ref('');
  let playerXName = ref('');
  let buttonDisabled = ref(true);
  let checkStartArr =ref<string[]>([]);

  const emits = defineEmits<{
    (e: string, name: string): void
  }>()

  const sendPlayerO = () => {
    emits('playerO', playerO.value);
    checkStartArr.value = [...checkStartArr.value, '1']
    playerOName.value = playerO.value;

    if(checkStartArr.value.length >= 2){
      enableStartGame()
    }
  }

  const sendPlayerX = () => {
    emits('playerX', playerX.value);
    checkStartArr.value = [...checkStartArr.value, '1'];
    playerXName.value = playerX.value;

    if(checkStartArr.value.length >= 2){
    enableStartGame()
  }
  }

  const enableStartGame = () => {
    console.log('runs')
    buttonDisabled.value = false;
  }

  const startGame = () => {
    emits('start', 'start');
  }

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