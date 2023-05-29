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

const emits = defineEmits<{
    (e: 'winner', winner: string): void
  }>()

let currentPlayer = ref(props.playerO);

let winner = ref('');

let haveWinner = ref(false)

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
  console.log('yes')
  if (haveWinner.value == true){
    console.log('nope')
    return
  } else{
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
    isWinner();
  }
}

const isWinner = () => {
  console.log('runs')
  if(
    (board.value[0].content === 'X' && board.value[1].content === 'X' && board.value[2].content === 'X') ||
    (board.value[3].content === 'X' && board.value[4].content === 'X' && board.value[5].content === 'X') ||
    (board.value[6].content === 'X' && board.value[7].content === 'X' && board.value[8].content === 'X') ||
    (board.value[0].content === 'X' && board.value[4].content === 'X' && board.value[8].content === 'X') ||
    (board.value[2].content === 'X' && board.value[4].content === 'X' && board.value[6].content === 'X')
  ){
    console.log('X wins')
    emits('winner', 'X');
    winner.value = props.playerX.name;
    haveWinner.value = true;

  } else if (
    (board.value[0].content === 'O' && board.value[1].content === 'O' && board.value[2].content === 'O') ||
    (board.value[3].content === 'O' && board.value[4].content === 'O' && board.value[5].content === 'O') ||
    (board.value[6].content === 'O' && board.value[7].content === 'O' && board.value[8].content === 'O') ||
    (board.value[0].content === 'O' && board.value[4].content === 'O' && board.value[8].content === 'O') ||
    (board.value[2].content === 'O' && board.value[4].content === 'O' && board.value[6].content === 'O')
  ){
    emits('winner', 'O');
    winner.value = props.playerO.name;
    haveWinner.value = true;

  } else if (
    board.value[0].content !== '' && board.value[1].content !== '' &&
    board.value[2].content !== '' && board.value[3].content !== '' && 
    board.value[4].content !== '' && board.value[5].content !== '' &&
    board.value[6].content !== '' && board.value[7].content !== '' &&
    board.value[8].content !== ''
  ) {
    emits('winner', 'even');
  } else{
    return
  }
}
</script>

<template>
  <h3 v-if="haveWinner">{{ winner }} won!</h3>
  <h3 v-else>It's {{ currentPlayer.name }}s turn</h3>
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