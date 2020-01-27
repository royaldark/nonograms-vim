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
                 :class="{filled: cell >= 0, selected: isCellSelected(idx), bounded: isInVisualModeBoundingBox(idx)}"
                 @click="clickCell(idx)">
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
        data() {
            return {
                selectedCell: -1,
                cellSize: 50,
                height: this.clues.horizontal.length,
                width: this.clues.vertical.length,
                grid: flatGrid(this.clues),
                visualMode: false,
                visualModeRootCell: null
            };
        },
        created() {
            window.addEventListener('keydown', this.handleKey);
        },
        destroyed() {
            window.removeEventListener('keydown', this.handleKey);
        },
        computed: {
            paletteStyle() {
                return {
                    height: `${0.75 * this.cellSize}px`,
                    width: `${this.cellSize * this.clues.vertical.length}px`,
                    'grid-template-columns': `repeat(${this.clues.palette.length}, 1fr)`,
                }
            },
            gridStyle() {
                return {
                    height: `${this.cellSize * this.clues.horizontal.length}px`,
                    width: `${this.cellSize * this.clues.vertical.length}px`,
                    'grid-template-rows': `repeat(${this.clues.horizontal.length}, 1fr)`,
                    'grid-template-columns': `repeat(${this.clues.vertical.length}, 1fr)`,
                }
            },
            visualModeBounds() {
                if (!this.visualMode) {
                    return null;
                }

                const [minX, maxX] = _.sortBy([
                    this.selectedCell % this.width,
                    this.visualModeRootCell % this.width,
                ]);

                const [minY, maxY] = _.sortBy([
                    Math.floor(this.selectedCell / this.width),
                    Math.floor(this.visualModeRootCell / this.width),
                ]);

                return { minX, maxX, minY, maxY };
            }
        },
        methods: {
            selectCell(idx) {
                this.selectedCell = idx;
            },
            clickCell(idx) {
                this.toggleCell(idx);
                this.selectCell(idx);
            },
            anyVisualModeBoxCellsShaded() {
                return _.keys(this.grid)
                    .filter(this.isInVisualModeBoundingBox)
                    .filter(this.isCellShaded)
                    .length > 0;
            },
            clearVisualModeBox() {
                _.keys(this.grid)
                    .filter(this.isInVisualModeBoundingBox)
                    .forEach(this.shadeCell);

                this.visualMode = false;
            },
            shadeVisualModeBox() {
                _.keys(this.grid)
                    .filter(this.isInVisualModeBoundingBox)
                    .forEach(this.clearCell);

            },
            toggleCell(idx) {
                if (this.visualMode) {
                    if (this.anyVisualModeBoxCellsShaded()) {
                        this.shadeVisualModeBox();
                    } else {
                        this.clearVisualModeBox();
                    }
                } else {
                    if (this.isCellShaded(idx)) {
                        this.clearCell(idx);
                    } else {
                        this.shadeCell(idx);
                    }
                }
            },
            isCellShaded(idx) {
                return this.grid[idx] > -1;
            },
            clearCell(idx) {
                this.grid.splice(idx, 1, -1);
            },
            shadeCell(idx) {
                this.grid.splice(idx, 1, 0);
            },
            isCellSelected(idx) {
                return this.selectedCell === idx;
            },
            isInVisualModeBoundingBox(idx) {
                if (!this.visualMode) {
                    return false;
                }

                const { minX, maxX, minY, maxY } = this.visualModeBounds;
                const [idxX, idxY] = [
                    idx % this.width,
                    Math.floor(idx / this.width)
                ];

                return (idxX >= minX && idxX <= maxX) &&
                    (idxY >= minY && idxY <= maxY);
            },
            handleKey(event) {
                console.log(event);

                if (event.key === " ") {
                    this.toggleCell(this.selectedCell);
                    this.visualMode = false;
                } else if (event.key === "x") {
                    this.clearCell(this.selectedCell);
                    this.visualMode = false;
                } else if (event.key === "h") {
                    if (this.selectedCell % this.width !== 0) {
                        this.selectCell(this.selectedCell - 1);
                    }
                } else if (event.key === "l") {
                    if (this.selectedCell % this.width !== this.width - 1) {
                        this.selectCell(this.selectedCell + 1);
                    }
                } else if (event.key === "j") {
                    if (this.selectedCell + this.width < this.grid.length) {
                        this.selectCell(this.selectedCell + this.width);
                    }
                } else if (event.key === "k") {
                    if (this.selectedCell - this.width >= 0) {
                        this.selectCell(this.selectedCell - this.width);
                    }
                } else if (event.key === "v") {
                    this.visualMode = !this.visualMode;

                    if (this.visualMode) {
                        this.visualModeRootCell = this.selectedCell;
                    }
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

        &.bounded {
            background-color: limegreen;
        }

        &.selected {
            background-color: red;
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