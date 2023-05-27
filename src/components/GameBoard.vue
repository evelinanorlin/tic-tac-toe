<script setup lang="ts">
import { ref } from 'vue';

const props = defineProps<{
  playerO: {
    name: string,
    score: number, 
    symbol: string
}, playerX: {
    name: string,
    score: number,
    symbol: string
}}>();

let currentPlayer = ref(props.playerO);

let board = ref([
  {id: 1, content: ''},
  {id: 2, content: ''},
  {id: 3, content: ''},
  {id: 4, content: ''},
  {id: 5, content: ''},
  {id: 6, content: ''},
  {id: 7, content: ''},
  {id: 8, content: ''},
  {id: 9, content: ''}
])

const squareClick = (currentId: number) => {

  board.value.map(square => {
    if (square.id == currentId){
      if(square.content !== ''){
        return
      }
      square.content = currentPlayer.value.symbol
      if (currentPlayer.value.symbol == 'O'){
        currentPlayer.value = props.playerX;
      } else{
        currentPlayer.value = props.playerO;
      }
    }
  })

}
</script>

<template>
  <p>It's {{ currentPlayer.name }}s turn</p>
  <div class="boardGrid">
    <div class="boardSquare" v-for="square in board" @click="() => squareClick(square.id)">{{ square.content }}</div>
  </div>
</template>

<style>
  .boardGrid{
    display: grid;
    grid-template-columns: 70px 70px 70px;
  }
  .boardSquare{
    height: 70px;
    width: 70px;
    border: 1px solid black;
  }
</style>