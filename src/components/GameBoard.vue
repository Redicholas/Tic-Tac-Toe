<script setup lang="ts">
import { IPlayer } from '../models/Player';
import { onMounted, ref } from 'vue';

const props = defineProps<{currentPlayer: IPlayer, playerX: IPlayer, playerO: IPlayer}>();
const emits = defineEmits<{(e: 'reset', empty: string): void}>();
const squares = ref<string[]>(['', '', '', '', '', '', '', '', '']);

let playerXturn = ref<boolean>(true);
let currentPlayer = ref<IPlayer>(props.playerX as IPlayer);

onMounted(() => {
    if (localStorage.getItem('squares')) {
        squares.value = JSON.parse(localStorage.getItem('squares') as string);
    }
    if (localStorage.getItem('playerXturn')) {
        playerXturn.value = JSON.parse(localStorage.getItem('playerXturn') as string);
    }
    if (localStorage.getItem('currentPlayer')) {
        currentPlayer.value = JSON.parse(localStorage.getItem('currentPlayer') as string);
    }
    if (localStorage.getItem('playerXscore')) {
        props.playerX.score = JSON.parse(localStorage.getItem('playerXscore') as string);
    }
    if (localStorage.getItem('playerOscore')) {
        props.playerO.score = JSON.parse(localStorage.getItem('playerOscore') as string);
    }
})

function saveState() {
    localStorage.setItem('squares', JSON.stringify(squares.value));
    localStorage.setItem('playerXturn', JSON.stringify(playerXturn.value));
    localStorage.setItem('currentPlayer', JSON.stringify(currentPlayer.value));
    localStorage.setItem('playerXscore', JSON.stringify(props.playerX.score));
    localStorage.setItem('playerOscore', JSON.stringify(props.playerO.score));
}

function handleClick(i: number) {
    if (squares.value[i] != '') return
    if (!weHaveAWinner()) {
        squares.value[i] = currentPlayer.value.symbol;
    } else {
        return;
    }
    if (gameIsEven()) alert(`The game is even, play again!`)
    if (weHaveAWinner()) {
        currentPlayer.value.score += 1;
        return;
    }
    playerXturn.value = !playerXturn.value;
    currentPlayer.value = playerXturn.value ? props.playerX : props.playerO;
    saveState();
}

function weHaveAWinner() {
    // rows
    if (squares.value[0] === squares.value[1] && squares.value[1] === squares.value[2] && squares.value[0] !== '' ||
        squares.value[3] === squares.value[4] && squares.value[4] === squares.value[5] && squares.value[3] !== '' ||
        squares.value[6] === squares.value[7] && squares.value[7] === squares.value[8] && squares.value[6] !== '') {
            switchTurn();
            saveState();
        return true;
    }

    // columns
    if (squares.value[0] === squares.value[3] && squares.value[3] === squares.value[6] && squares.value[0] !== '' ||
        squares.value[1] === squares.value[4] && squares.value[4] === squares.value[7] && squares.value[1] !== '' ||
        squares.value[2] === squares.value[5] && squares.value[5] === squares.value[8] && squares.value[2] !== '') {
            switchTurn();
            saveState();
        return true;
    }

    // diagonals
    if (squares.value[0] === squares.value[4] && squares.value[4] === squares.value[8] && squares.value[0] !== '' ||
        squares.value[2] === squares.value[4] && squares.value[4] === squares.value[6] && squares.value[2] !== '') {
            switchTurn();
            saveState();
        return true;
    }

    return false;
}

function switchTurn() {
    playerXturn.value = currentPlayer.value === props.playerX ? false : true;
}

function gameIsEven() {
    return squares.value.every(square => square !== '') && !weHaveAWinner();
}

function playAgain() {
    squares.value = ['', '', '', '', '', '', '', '', ''];
    currentPlayer.value = playerXturn.value ? props.playerX : props.playerO;
    saveState();
}

function reset() {
    squares.value = ['', '', '', '', '', '', '', '', ''];
    localStorage.clear();
    emits('reset', '');
}

</script>

<template>
    <h2 v-if="weHaveAWinner()">{{ currentPlayer.name }} won the game! 
        <br />Play again</h2>
    <h2 v-else-if="gameIsEven()">The game is even, play again!</h2>
    <h2 v-else >{{ currentPlayer.name }}'s Turn</h2>
    <main>
        <div class="grid">
            <div class="square" v-for="(square, i) in squares"
            :key="i"
            @click="() => handleClick(i)">
            {{ square }}
            </div>
        </div>
        <div class="stats">
            <h3>Score</h3>
            <p>{{ props.playerX.name }}: {{ props.playerX.score }}</p>
            <p>{{ props.playerO.name }}: {{ props.playerO.score }}</p>
            <button @click="playAgain">Play Again</button>
            <button @click="reset">Reset</button>
        </div>
    </main>
</template>

<style scoped>
    main {
        display: flex;
        justify-content: space-around;
        align-items: center;
        gap: 2rem;
    }

    .stats {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        gap: 5px;
    }

    .grid {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        width: 15rem;
        height: 15rem;
        background-color: rgb(0, 0, 0);
        margin: 1rem auto;
    }

    .square {
        width: 5rem;
        height: 5rem;
        border: 1px solid rgb(255, 255, 255);
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 5rem;
    }

</style>