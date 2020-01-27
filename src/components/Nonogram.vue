<template>
    <section>
        <Palette :clues="clues" :cell-size="cellSize"></Palette>
        <TopHints :clues="clues" :cell-size="cellSize"></TopHints>
        <Grid :clues="clues" :cell-size="cellSize"></Grid>
    </section>
</template>

<script>
    import Grid from './Grid.vue'
    import Palette from './Palette.vue'
    import TopHints from './TopHints.vue'

    export default {
        components: {
            Grid,
            Palette,
            TopHints,
        },
        props: {
            clues: Object
        },
        data() {
            return {
                cellSize: 50,
            };
        },
    }
</script>

<style scoped lang="scss">
</style>