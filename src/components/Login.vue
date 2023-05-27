<script setup lang="ts">
import { ref } from 'vue';

const emits = defineEmits<{(e: 'getPlayers', nameX: string, nameO: string): void}>()

function getPlayers() {
    emits("getPlayers", playerXname.value, playerOname.value);   
}

const playerXname = ref<string>('');
const playerOname = ref<string>('');

</script>

<template>
    <input type="text" v-model="playerXname" placeholder="Player Xs Name">
    <input type="text" v-model="playerOname" placeholder="Player Os Name">
    <button @click="getPlayers">Start Game</button>
</template>

<style scoped></style>