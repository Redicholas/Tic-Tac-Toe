<script setup lang="ts">
import { IPlayer } from '../models/Player';
import { ref } from 'vue';

const props = defineProps<{currentPlayer: IPlayer}>();

defineEmits<{(e: 'handleClick'): void}>()

const squares = ref<string[]>(['', '', '', '', '', '', '', '', '']);

</script>

<template>
    <div class="grid">
        <div class="square" v-for="(square, i) in squares">{{ i }}</div>
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
    }

</style>