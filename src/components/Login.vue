<script setup lang="ts">
import { ref } from 'vue';

const emits = defineEmits<{(e: 'getPlayers', nameX: string, nameO: string): void}>()

function getPlayers() {
    if (playerXname.value === '' || playerOname.value === '') {
        alert('Please enter both players names');
        return;
    }
    emits("getPlayers", playerXname.value, playerOname.value);   
}

const playerXname = ref<string>('');
const playerOname = ref<string>('');

</script>

<template>
    <div class="login">
        <input type="text" v-model="playerXname" placeholder="Player Xs Name">
        <input type="text" v-model="playerOname" placeholder="Player Os Name">
        <button @click="getPlayers">Start Game</button>
    </div>
</template>

<style scoped>
    input {
        height: 30px;
    }
    .login {
        display: flex;
        flex-direction: column;
        gap: 10px;
    }
</style>