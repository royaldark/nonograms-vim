<template>
    <section>
        <div>This a grid</div>

        <div class="palette" :style="paletteStyle">
            <div v-for="(color, idx) in clues.palette"
                 :key="idx"
                 class="swatch"
                 :style="{'background-color': color}">
                <span>&nbsp;</span>
            </div>
        </div>

        <div class="grid" :style="gridStyle">
            <div v-for="(cell, idx) in grid"
                 :key="idx"
                 class="cell"
                 :class="{filled: cell >= 0}"
                 @click="toggleCell(idx)">
                <div class="cell-inner">
                    <span>&nbsp;</span>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
    import * as _ from 'lodash'

    /*const emptyGrid = (clues) => {
        const row = Array(clues.horizontal.length).fill(0);

        return Array(clues.vertical.length).fill(0).map(() => {
            return _.clone(row);
        });
    };*/

    const flatGrid = (clues) => {
        return Array(clues.horizontal.length * clues.vertical.length).fill(-1);
    };

    export default {
        props: {
            clues: Object
        },
        computed: {
            paletteStyle() {
                return {
                    height: `${0.75 * this.cellSize}px`,
                    width: `${this.cellSize * this.clues.horizontal.length}px`,
                    'grid-template-columns': `repeat(${this.clues.palette.length}, 1fr)`,
                }
            },
            gridStyle() {
                return {
                    height: `${this.cellSize * this.clues.vertical.length}px`,
                    width: `${this.cellSize * this.clues.horizontal.length}px`,
                    'grid-template-rows': `repeat(${this.clues.horizontal.length}, 1fr)`,
                    'grid-template-columns': `repeat(${this.clues.vertical.length}, 1fr)`,
                }
            }
        },
        data() {
            return {
                cellSize: 50,
                grid: flatGrid(this.clues)
            };
        },
        methods: {
            toggleCell(idx) {
                const cur = this.grid[idx];

                if (cur === 0) {
                    this.grid.splice(idx, 1, 1);
                } else {
                    this.grid.splice(idx, 1, 0);
                }
            }
        }
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

    .grid {
        border-radius: 10px;
        display: grid;
        grid-gap: 2px;
        background-color: black;
        border: 2px solid black;
        padding: 2px;
    }

    .cell {
        //border: 2px solid black;
        cursor: pointer;
        background-color: white;
        display: flex; flex-direction: column;

        &:hover {
            background-color: lightgray;
        }

        .cell-inner {
            background-color: white;
            margin: 2px;
            flex-grow: 1;
        }

        &.filled {
            .cell-inner {
                background-color: black;
            }
        }
    }

</style>