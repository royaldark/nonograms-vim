<template>
    <div id="app">
        <label>
            Pick a game:

            <select v-model="clues">
                <option v-for="(game, name) in games" :key="name" :value="game">
                    {{ name }}
                </option>
            </select>
        </label>

        <Nonogram :clues="clues"></Nonogram>
    </div>
</template>

<script>
    import Nonogram from './components/Nonogram.vue'
    import * as Games from './lib/game.js'

    export default {
        name: 'app',
        components: {
            Nonogram,
        },
        data() {
            return {
                games: Games,
                clues: Games.snowflake
            };
        }
    }
</script>

<style lang="scss">
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
