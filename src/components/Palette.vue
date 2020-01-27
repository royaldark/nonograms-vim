<template>
    <div class="palette" :style="paletteStyle">
        <div v-for="(color, idx) in clues.palette"
             :key="idx"
             class="swatch"
             :style="{'background-color': color}">
            <span>&nbsp;</span>
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            clues: Object,
            cellSize: Number
        },
        data() {
            return {
            };
        },
        computed: {
            height() {
                return this.clues.horizontal.length;
            },
            width() {
                return this.clues.vertical.length;
            },
            paletteStyle() {
                return {
                    height: `${0.75 * this.cellSize}px`,
                    width: `${this.cellSize * this.width}px`,
                    'grid-template-columns': `repeat(${this.clues.palette.length}, 1fr)`,
                }
            },
        },
    }
</script>

<style scoped lang="scss">
    .palette {
        border-radius: 10px;
        display: grid;
        grid-gap: 2px;
        background-color: black;
        border: 2px solid black;
        padding: 2px;
        margin-bottom: 1rem;
    }

    .swatch {
        cursor: pointer;
    }
</style>