<template>
    <div id="app">
        <img alt="Vue logo" src="./assets/logo.png">
        <Grid :clues="clues"></Grid>
    </div>
</template>

<script>
    import Grid from './components/Grid.vue'
    import * as Game from './lib/game.js'

    export default {
        name: 'app',
        components: {
            Grid,
        },
        data() {
            return {
                clues: Game.big
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
