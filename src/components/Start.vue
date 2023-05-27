<script setup lang="ts">
import { ref } from 'vue';
import Login from './Login.vue';

let showLogin = ref(true);

let playerO = ref({name: '', score: 0})
let playerX = ref({name: '', score: 0})

const addPlayerO = (newName: string) => {
  playerO.value.name = newName;
  console.log(playerO);
}

const addPlayerX = (newName: string) => {
  playerX.value.name = newName;
  console.log(playerX);
}
</script>

<template>
  <div v-if="showLogin">
    <Login @playerO="addPlayerO" @playerX="addPlayerX"/>
  </div>
</template>