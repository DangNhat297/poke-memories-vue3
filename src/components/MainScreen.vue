<template>
    <div class="screen">
        <h1>Poke Memories</h1>
        <h3>Select mode to play game</h3>
        <div class="modes">
            <button @click="onStart(16)">
                <span>4x4</span>
                <span>Easy</span>
            </button>
            <button @click="onStart(36)">
                <span>6x6</span>
                <span>Normal</span>
            </button>
            <button @click="onStart(64)">
                <span>8x8</span>
                <span>Hard</span>
            </button>
            <button @click="onStart(100)">
                <span>10x10</span>
                <span>Super Hard</span>
            </button>
        </div>
    </div>
    <copyright-screen />
</template>

<script>
import CopyrightScreen from './CopyrightScreen.vue'
export default {
  components: { CopyrightScreen },
    methods: {
        onStart(totalOfBlock) {
            this.$emit('onStart', { totalOfBlock })
        }
    },
}
</script>

<style lang="css" scoped>
    .screen {
        width: 100%;
        height: 100vh;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        color: var(--light);
        text-align: center;
        row-gap: 20px;
    }

    .screen h1 {
        text-transform: uppercase;
        font-size: 4rem;
    } 

    .screen h3 {
        font-size: 2rem;
    }

    .modes {
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        grid-gap: 30px;
    }

    .modes button {
        width: 150px;
        aspect-ratio: 1/1;
        outline: none;
        border: 2px solid var(--light);
        background: transparent;
        display: flex;
        flex-direction: column;
        font-family: var(--font);
        cursor: pointer;
        align-items: center;
        justify-content: center;
        color: var(--light);
        font-size: 1.7rem;
        border-radius: 1.5rem;
        transition: color .2s linear, background .2s linear;
    }

    .modes button:hover {
        background: var(--light);
        color: var(--dark);
    }
</style>