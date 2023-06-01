<script setup lang="ts">
import { ref } from 'vue';

const props = defineProps<{
  playerO: {name: string, score: number, symbol: string}, 
  playerX: {name: string, score: number, symbol: string
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
    board = ref([
    {id: 1, content: '', class: false}, {id: 2, content: '', class: false}, {id: 3, content: '', class: false},
    {id: 4, content: '', class: false}, {id: 5, content: '', class: false}, {id: 6, content: '', class: false},
    {id: 7, content: '', class: false}, {id: 8, content: '', class: false}, {id: 9, content: '', class: false}
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

const updateState = () => {
  localStorage.setItem('winner', JSON.stringify(winner.value));
  localStorage.setItem('haveWinner', JSON.stringify(haveWinner.value));
  localStorage.setItem('isEven', JSON.stringify(isEven.value));
  localStorage.setItem('board', JSON.stringify(board.value)); 
  localStorage.setItem('isEven', JSON.stringify(isEven.value));
  localStorage.setItem('currentPlayer', JSON.stringify(currentPlayer.value));
}

const squareClick = (currentId: number) => {
  if (haveWinner.value == true){
    return
  } else{
    board.value.map((square: { id: number; content: string; class: boolean}) => {
      if (square.id == currentId){
        if(square.content !== ''){
          return
        }
        square.content = currentPlayer.value.symbol;
        square.class = true;
        updateState();
        if (currentPlayer.value.symbol == 'O'){
          currentPlayer.value = props.playerX;
          updateState();
        } else{
          currentPlayer.value = props.playerO;
          updateState();
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
    board.value.map((square: {class: boolean}) => {
      square.class = true;
    })
    updateState();
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
    board.value.map((square: {class: boolean}) => {
      square.class = true;
    })
    updateState()
  } else if (
    board.value[0].content !== '' && board.value[1].content !== '' &&
    board.value[2].content !== '' && board.value[3].content !== '' && 
    board.value[4].content !== '' && board.value[5].content !== '' &&
    board.value[6].content !== '' && board.value[7].content !== '' &&
    board.value[8].content !== ''
  ) {
    emits('winner', 'even');
    isEven.value = true;
    updateState();
  } else{
    return
  }
}

const resetBoard = () => {
  board.value.map((square: { content: string; class: boolean }) => {
    square.content = '';
    square.class = false;
  })
  haveWinner.value = false; 
  isEven.value = false;
  updateState();
}

getStartValues();
</script>

<template>
  <h2 v-if="haveWinner && !isEven">{{ winner }} won!</h2>
  <h2 v-else-if="!haveWinner && isEven">Let´s call it even</h2>
  <h2 v-else>It's {{ currentPlayer.name }}s turn</h2>
  <div class="boardGrid">
    <div v-for="square in board" @click="() => squareClick(square.id)">
        <div  class="boardSquare" :class="{taken: square.class}">
        <p class="symbol">{{ square.content }}</p>
      </div>
    </div>
  </div>
  <button @click="resetBoard" class="playAgainBtn">Play again</button>
</template>

<style>
  .boardGrid{
    display: grid;
    grid-template-columns: 100px 100px 100px;
    margin-right: 60px;
  }
  .boardSquare{
    height: 100px;
    width: 100px;
    border: 1px solid black;
    cursor: pointer;
    transition: background .3s;
    background: #edd1ac;
  }

  .boardSquare:hover{
    background: #8f7e67;
  }

  .taken{
    cursor: default;
  }

  .taken:hover{
    background: #edd1ac;
  }

  .symbol{
    font-size: 7rem;
    margin: 0;
    position: relative;
    bottom: 15px;
    left: 7px;
  }

  .playAgainBtn{
    margin-top: 20px;
  }
</style>