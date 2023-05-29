<script setup lang="ts">
import { ref } from 'vue';
import { IPlayer } from '../models/Player';
import Login from './Login.vue';
import GameBoard from './GameBoard.vue';

let gameIsRunning = ref<boolean>(false);
let playerXturn = ref<boolean>(true);
let currentPlayer = ref<IPlayer>({} as IPlayer);

let playerX = ref<IPlayer>({ name: '', symbol: 'X', score: 0 });
let playerO = ref<IPlayer>({ name: '', symbol: 'O', score: 0 });

if(playerXturn.value) {
    currentPlayer.value = playerX.value;
    console.log(currentPlayer.value);
    
} else {
    currentPlayer.value = playerO.value;
    console.log(currentPlayer.value);
}

function handleGetPlayers(playerXname: string, playerOname: string) {
    playerX.value.name = playerXname;
    playerO.value.name = playerOname;
    gameIsRunning.value = true;
    console.log(playerX.value, playerO.value);
    
}

</script>

<template>
    <Login v-if="!gameIsRunning" @getPlayers="handleGetPlayers" />
    <GameBoard v-else :currentPlayer="currentPlayer" />
</template>

<style scoped></style>