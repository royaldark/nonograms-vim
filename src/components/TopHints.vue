<template>
    <div class="top-hints" :style="topHintsStyle">
        <div v-for="(hints, colIdx) in clues.vertical" :key="colIdx" class="hint-column">
            <div v-for="(hint, hintIdx) in hints"
                 :key="hintIdx"
                 :style="hintStyle(hint)"
                 class="hint">
                {{ hint[1] }}
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            clues: Object,
            cellSize: Number,
        },
        computed: {
            height() {
                return this.clues.horizontal.length;
            },
            width() {
                return this.clues.vertical.length;
            },
            topHintsStyle() {
                return {
                    width: `${this.cellSize * this.width}px`,
                    'grid-template-columns': `repeat(${this.width}, 1fr)`,
                }
            },
        },
        methods: {
            hintStyle(hint) {
                return {
                    'background-color': this.clues.palette[hint[0]],
                    height: `${this.cellSize}px`
                }
            },
        }
    }
</script>

<style scoped lang="scss">
    .top-hints {
        display: grid;
        grid-gap: 2px;
        border: 2px solid black;
        padding: 2px;
        margin-bottom: 0.25rem;
    }

    .hint-column {
        display: flex;
        flex-direction: column;
        justify-content: flex-end;
        color: white;
        font-weight: bold;
    }

    .hint {
        display: flex;
        justify-content: center;
    }
</style>