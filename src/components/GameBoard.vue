<script setup lang="ts">
import { IPlayer } from '../models/Player';
import { ref } from 'vue';

const props = defineProps<{currentPlayer: IPlayer, playerX: IPlayer, playerO: IPlayer}>();
const squares = ref<string[]>(['', '', '', '', '', '', '', '', '']);

let playerXturn = ref<boolean>(true);
let currentPlayer = ref<IPlayer>(props.playerX as IPlayer);

function handleClick(i: number) {
  currentPlayer.value = playerXturn.value ? props.playerX : props.playerO;
  squares.value[i] = currentPlayer.value.symbol;
  if (weHaveAWinner()) {
    alert(`${currentPlayer.value.name} wins!`);
    squares.value = ['', '', '', '', '', '', '', '', ''];
    return;
  }
  playerXturn.value = !playerXturn.value;
  currentPlayer.value = playerXturn.value ? props.playerX : props.playerO;
}

function weHaveAWinner() {
    // rows
    if (squares.value[0] === squares.value[1] && squares.value[1] === squares.value[2] && squares.value[0] !== '') {
        return true;
    }
    if (squares.value[3] === squares.value[4] && squares.value[4] === squares.value[5] && squares.value[3] !== '') {
        return true;
    }
    if (squares.value[6] === squares.value[7] && squares.value[7] === squares.value[8] && squares.value[6] !== '') {
        return true;
    }
    // columns
    if (squares.value[0] === squares.value[3] && squares.value[3] === squares.value[6] && squares.value[0] !== '') {
        return true;
    }
    if (squares.value[1] === squares.value[4] && squares.value[4] === squares.value[7] && squares.value[1] !== '') {
        return true;
    }
    if (squares.value[2] === squares.value[5] && squares.value[5] === squares.value[8] && squares.value[2] !== '') {
        return true;
    }
    // diagonals
    if (squares.value[0] === squares.value[4] && squares.value[4] === squares.value[8] && squares.value[0] !== '') {
        return true;
    }
    if (squares.value[2] === squares.value[4] && squares.value[4] === squares.value[6] && squares.value[2] !== '') {
        return true;
    }
    return false;
}

</script>

<template>
    <!-- TODO: Display winner text -->
    <h2>{{ currentPlayer.name }}'s Turn</h2>
    <div class="grid">
        <div class="square" v-for="(square, i) in squares"
             :key="i"
             @click="() => handleClick(i)">
             {{ square }}
        </div>
    </div>
</template>

<style scoped>

    .grid {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        width: 150px;
        height: 150px;
        background-color: rgb(0, 0, 0);
        margin: 0 auto;
    }

    .square {
        width: 50px;
        height: 50px;
        border: 1px solid rgb(255, 255, 255);
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 30px;
    }

</style>