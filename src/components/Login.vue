<script setup lang="ts">
  import { ref } from 'vue'; 

  let playerO = ref('');
  let playerX = ref('');
  let buttonDisabled = ref(true);
  let checkStartArr =ref<string[]>([]);

  const emits = defineEmits<{
    (e: string, name: string): void
  }>()

  const sendPlayerO = () => {
    emits('playerO', playerO.value);
    checkStartArr.value = [...checkStartArr.value, '1']

    if(checkStartArr.value.length >= 2){
      enableStartGame()
    }
  }

  const sendPlayerX = () => {
    emits('playerX', playerX.value);
    checkStartArr.value = [...checkStartArr.value, '1']

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

  <form>
    <label for="playerO">Player O</label><br>
    <input type="text" id="playerO" v-model="playerO">
    <button @click.prevent="sendPlayerO">Spara</button>
  </form>

  <form>
    <label for="playerX">Player X</label><br>
    <input type="text" id="playerX" v-model="playerX">
    <button @click.prevent="sendPlayerX">Spara</button>
  </form>

  <button :disabled="buttonDisabled" @click="startGame">Start game!</button>
</template>