<script setup lang="ts">
import { ref } from 'vue';
import { IPlayer } from '../models/Player';
import Login from './Login.vue';
import GameBoard from './GameBoard.vue';

let gameIsRunning = localStorage.getItem('gameIsRunning') ? ref<boolean>(JSON.parse(localStorage.getItem('gameIsRunning') as string)) : ref<boolean>(false);
let playerX = localStorage.getItem('playerX') ? ref<IPlayer>(JSON.parse(localStorage.getItem('playerX') as string)) : ref<IPlayer>({ name: '', symbol: 'X', score: 0 });
let playerO = localStorage.getItem('playerO') ? ref<IPlayer>(JSON.parse(localStorage.getItem('playerO') as string)) : ref<IPlayer>({ name: '', symbol: 'O', score: 0 });

function handleGetPlayers(playerXname: string, playerOname: string) {
    playerX.value.name = playerXname;
    playerO.value.name = playerOname;
    gameIsRunning.value = true;
    localStorage.setItem('gameIsRunning', JSON.stringify(true));
    localStorage.setItem('playerX', JSON.stringify(playerX.value));
    localStorage.setItem('playerO', JSON.stringify(playerO.value));
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