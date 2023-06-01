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

let board = ref(JSON.parse(localStorage.getItem('board') ?? 'null'));
let currentPlayer = ref(JSON.parse(localStorage.getItem('currentPlayer') ?? 'null'));
let winner = ref(JSON.parse(localStorage.getItem('winner') ?? 'null'));
let haveWinner = ref(JSON.parse(localStorage.getItem('haveWinner') ?? '"noWinner"'));
let isEven = ref(JSON.parse(localStorage.getItem('isEven') ?? '"notEven"'));

const getStartValues = () => {
  if(!board.value){
    board = ref([{id: 1, content: ''}, {id: 2, content: ''}, {id: 3, content: ''},
    {id: 4, content: ''}, {id: 5, content: ''}, {id: 6, content: ''},
    {id: 7, content: ''}, {id: 8, content: ''}, {id: 9, content: ''}
  ]);
  }
  if(!currentPlayer.value){
    currentPlayer = ref(props.playerO);
  }
  if(!winner){
    winner = ref('');
  }
  if(haveWinner.value === "noWinner"){
    haveWinner = ref(false);
  }
  if(isEven.value === "notEven"){
    isEven = ref(false);
  }
}

const squareClick = (currentId: number) => {
  if (haveWinner.value == true){
    return
  } else{
    board.value.map((square: { id: number; content: string; }) => {
      if (square.id == currentId){
        if(square.content !== ''){
          return
        }
        square.content = currentPlayer.value.symbol;
        localStorage.setItem('board', JSON.stringify(board.value));
        if (currentPlayer.value.symbol == 'O'){
          currentPlayer.value = props.playerX;
          localStorage.setItem('currentPlayer', JSON.stringify(currentPlayer.value));
        } else{
          currentPlayer.value = props.playerO;
          localStorage.setItem('currentPlayer', JSON.stringify(currentPlayer.value));
        }
      }
    })
    isWinner();
  }
}

const isWinner = () => {
  if(
    board.value[0].content === 'X' && board.value[1].content === 'X' && board.value[2].content === 'X' ||
    board.value[3].content === 'X' && board.value[4].content === 'X' && board.value[5].content === 'X' ||
    board.value[6].content === 'X' && board.value[7].content === 'X' && board.value[8].content === 'X' ||
    board.value[0].content === 'X' && board.value[4].content === 'X' && board.value[8].content === 'X' ||
    board.value[2].content === 'X' && board.value[4].content === 'X' && board.value[6].content === 'X' ||
    board.value[0].content === 'X' && board.value[3].content === 'X' && board.value[6].content === 'X' ||
    board.value[1].content === 'X' && board.value[4].content === 'X' && board.value[7].content === 'X' ||
    board.value[2].content === 'X' && board.value[5].content === 'X' && board.value[8].content === 'X'
  ){
    emits('winner', 'X');
    winner.value = props.playerX.name;
    haveWinner.value = true;
    localStorage.setItem('winner', JSON.stringify(winner.value));
    localStorage.setItem('haveWinner', JSON.stringify(haveWinner.value));
  } else if (
    board.value[0].content === 'O' && board.value[1].content === 'O' && board.value[2].content === 'O' ||
    board.value[3].content === 'O' && board.value[4].content === 'O' && board.value[5].content === 'O' ||
    board.value[6].content === 'O' && board.value[7].content === 'O' && board.value[8].content === 'O' ||
    board.value[0].content === 'O' && board.value[4].content === 'O' && board.value[8].content === 'O' ||
    board.value[2].content === 'O' && board.value[4].content === 'O' && board.value[6].content === 'O' ||
    board.value[0].content === 'O' && board.value[3].content === 'O' && board.value[6].content === 'O' ||
    board.value[1].content === 'O' && board.value[4].content === 'O' && board.value[7].content === 'O' ||
    board.value[2].content === 'O' && board.value[5].content === 'O' && board.value[8].content === 'O'
    ){
    emits('winner', 'O');
    winner.value = props.playerO.name;
    haveWinner.value = true;
    localStorage.setItem('winner', JSON.stringify(winner.value));
    localStorage.setItem('haveWinner', JSON.stringify(haveWinner.value));
  } else if (
    board.value[0].content !== '' && board.value[1].content !== '' &&
    board.value[2].content !== '' && board.value[3].content !== '' && 
    board.value[4].content !== '' && board.value[5].content !== '' &&
    board.value[6].content !== '' && board.value[7].content !== '' &&
    board.value[8].content !== ''
  ) {
    emits('winner', 'even');
    isEven.value = true;
    localStorage.setItem('isEven', JSON.stringify(isEven.value));
  } else{
    return
  }
}

const resetBoard = () => {
  board.value.map((square: { content: string; }) => {
    square.content = '';
  })
  haveWinner.value = false; 
  isEven.value = false;
  localStorage.setItem('haveWinner', JSON.stringify(haveWinner.value));
  localStorage.setItem('isEven', JSON.stringify(isEven.value));
  localStorage.setItem('board', JSON.stringify(board.value));
}

getStartValues();
</script>

<template>
  <h3 v-if="haveWinner && !isEven">{{ winner }} won!</h3>
  <h3 v-else-if="!haveWinner && isEven">Let´s call it even</h3>
  <h3 v-else>It's {{ currentPlayer.name }}s turn</h3>
  <div class="boardGrid">
    <div class="boardSquare" v-for="square in board" @click="() => squareClick(square.id)"><p class="symbol">{{ square.content }}</p></div>
  </div>
  <button @click="resetBoard">Play again</button>
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
  .symbol{
    font-size: 5rem;
    margin: 0;
    position: relative;
    bottom: 10px;
    left: 7px;
  }
</style>