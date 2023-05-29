<script setup lang="ts">
import { IPlayer } from '../models/Player';
import { ref } from 'vue';

const props = defineProps<{currentPlayer: IPlayer, playerX: IPlayer, playerO: IPlayer}>();
const squares = ref<string[]>(['', '', '', '', '', '', '', '', '']);

let playerXturn = ref<boolean>(true);
let currentPlayer = ref<IPlayer>(props.playerX as IPlayer);

currentPlayer.value = playerXturn.value ? props.playerX : props.playerO;

function handleClick(i: number) {
    currentPlayer.value = playerXturn.value ? props.playerX : props.playerO;
    squares.value[i] = currentPlayer.value.symbol;
    playerXturn.value = !playerXturn.value;
}

</script>

<template>
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