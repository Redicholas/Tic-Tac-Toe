<script setup lang="ts">
import { ref } from 'vue';
import { IPlayer } from '../models/Player';
import Login from './Login.vue';

const gameIsRunning = ref<boolean>(false);
const playerX = ref<IPlayer>({ name: '', symbol: 'X', score: 0 });
const playerO = ref<IPlayer>({ name: '', symbol: 'O', score: 0 });



function handleGetPlayers(playerXname: string, playerOname: string) {
    playerX.value.name = playerXname;
    playerO.value.name = playerOname;
}

</script>

<template>
    <Login v-if="!gameIsRunning" @getPlayers="handleGetPlayers" />
</template>

<style scoped></style>