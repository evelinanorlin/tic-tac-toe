<script setup lang="ts">
  import { ref } from 'vue'; 

  let playerO = ref('');
  let playerX = ref('');

  const emits = defineEmits<{
    (e: string, name: string): void
  }>()

  const sendPlayerO = () => {
    emits('playerO', playerO.value);
  }

  const sendPlayerX = () => {
    emits('playerX', playerX.value);
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

  <button disabled>Start game!</button>
</template>