<script setup lang="ts">
import { ref } from 'vue';
import { IPlayer } from '../models/Player';
import Login from './Login.vue';
import GameBoard from './GameBoard.vue';


let gameIsRunning = ref<boolean>(false);

let playerX = ref<IPlayer>({ name: '', symbol: 'X', score: 0 });
let playerO = ref<IPlayer>({ name: '', symbol: 'O', score: 0 });

function handleGetPlayers(playerXname: string, playerOname: string) {
    playerX.value.name = playerXname;
    playerO.value.name = playerOname;
    gameIsRunning.value = true;
    // console.log(playerX.value, playerO.value);
}

function reset() {
    gameIsRunning.value = false;
    playerX.value = { name: '', symbol: 'X', score: 0 };
    playerO.value = { name: '', symbol: 'O', score: 0 };
}

</script>

<template>
    <GameBoard v-if="gameIsRunning" :playerX="playerX" :playerO="playerO" @reset="reset" />
    <Login v-else @getPlayers="handleGetPlayers" />
</template>

<style scoped></style>