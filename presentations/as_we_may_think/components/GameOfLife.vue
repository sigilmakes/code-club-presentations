<template>
    <canvas
        ref="canvas"
        :width="canvasWidth"
        :height="canvasHeight"
        @click="toggleCell"
        class="game-canvas"
    />
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps({
    cols: { type: Number, default: 60 },
    rows: { type: Number, default: 30 },
    cellSize: { type: Number, default: 14 },
    interval: { type: Number, default: 150 },
    aliveColor: { type: String, default: '#955438' },
    deadColor: { type: String, default: '#201e15' },
    gridColor: { type: String, default: '#2a2820' },
})

const canvas = ref(null)
const canvasWidth = props.cols * props.cellSize
const canvasHeight = props.rows * props.cellSize

let grid = []
let running = false
let timer = null

function createGrid() {
    return Array.from({ length: props.rows }, () =>
        Array.from({ length: props.cols }, () => Math.random() < 0.3 ? 1 : 0)
    )
}

function countNeighbors(g, r, c) {
    let count = 0
    for (let dr = -1; dr <= 1; dr++) {
        for (let dc = -1; dc <= 1; dc++) {
            if (dr === 0 && dc === 0) continue
            const nr = (r + dr + props.rows) % props.rows
            const nc = (c + dc + props.cols) % props.cols
            count += g[nr][nc]
        }
    }
    return count
}

function step() {
    const next = grid.map((row, r) =>
        row.map((cell, c) => {
            const n = countNeighbors(grid, r, c)
            if (cell) return (n === 2 || n === 3) ? 1 : 0
            return n === 3 ? 1 : 0
        })
    )
    grid = next
    draw()
}

function draw() {
    const ctx = canvas.value?.getContext('2d')
    if (!ctx) return

    ctx.fillStyle = props.deadColor
    ctx.fillRect(0, 0, canvasWidth, canvasHeight)

    // Draw grid lines
    ctx.strokeStyle = props.gridColor
    ctx.lineWidth = 0.5
    for (let r = 0; r <= props.rows; r++) {
        ctx.beginPath()
        ctx.moveTo(0, r * props.cellSize)
        ctx.lineTo(canvasWidth, r * props.cellSize)
        ctx.stroke()
    }
    for (let c = 0; c <= props.cols; c++) {
        ctx.beginPath()
        ctx.moveTo(c * props.cellSize, 0)
        ctx.lineTo(c * props.cellSize, canvasHeight)
        ctx.stroke()
    }

    // Draw alive cells
    for (let r = 0; r < props.rows; r++) {
        for (let c = 0; c < props.cols; c++) {
            if (grid[r][c]) {
                ctx.fillStyle = props.aliveColor
                ctx.fillRect(
                    c * props.cellSize + 1,
                    r * props.cellSize + 1,
                    props.cellSize - 2,
                    props.cellSize - 2
                )
            }
        }
    }
}

function toggleCell(e) {
    const rect = canvas.value.getBoundingClientRect()
    const c = Math.floor((e.clientX - rect.left) / props.cellSize)
    const r = Math.floor((e.clientY - rect.top) / props.cellSize)
    if (r >= 0 && r < props.rows && c >= 0 && c < props.cols) {
        grid[r][c] = grid[r][c] ? 0 : 1
        draw()
    }
}

onMounted(() => {
    grid = createGrid()
    draw()
    timer = setInterval(step, props.interval)
    running = true
})

onUnmounted(() => {
    if (timer) clearInterval(timer)
})
</script>

<style scoped>
.game-canvas {
    display: block;
    margin: 0 auto;
    border-radius: 4px;
}
</style>
