<template>
  <div class="flex flex-col items-center gap-4">
    <canvas
      ref="canvas"
      :width="cols * cellSize"
      :height="rows * cellSize"
      class="border border-gray-600 rounded cursor-pointer"
      @click="toggleCell"
    />
    <div class="flex gap-3 items-center">
      <button
        class="px-4 py-1 rounded text-sm font-mono"
        :class="running ? 'bg-red-700 hover:bg-red-600' : 'bg-emerald-700 hover:bg-emerald-600'"
        @click="running ? stop() : start()"
      >
        {{ running ? '⏹ Stop' : '▶ Play' }}
      </button>
      <button
        class="px-4 py-1 rounded bg-gray-700 hover:bg-gray-600 text-sm font-mono"
        @click="step"
        :disabled="running"
      >
        Step
      </button>
      <button
        class="px-4 py-1 rounded bg-gray-700 hover:bg-gray-600 text-sm font-mono"
        @click="randomize"
      >
        Randomize
      </button>
      <button
        class="px-4 py-1 rounded bg-gray-700 hover:bg-gray-600 text-sm font-mono"
        @click="clear"
      >
        Clear
      </button>
      <span class="text-xs opacity-50 font-mono ml-2">Gen: {{ generation }}</span>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, watch } from 'vue'

const cols = 60
const rows = 25
const cellSize = 11

const canvas = ref(null)
const running = ref(false)
const generation = ref(0)
let grid = createGrid()

function createGrid() {
  return Array.from({ length: rows }, () => Array(cols).fill(0))
}

function randomize() {
  stop()
  generation.value = 0
  grid = Array.from({ length: rows }, () =>
    Array.from({ length: cols }, () => (Math.random() < 0.3 ? 1 : 0))
  )
  draw()
}

function clear() {
  stop()
  generation.value = 0
  grid = createGrid()
  draw()
}

function countNeighbors(g, r, c) {
  let count = 0
  for (let dr = -1; dr <= 1; dr++) {
    for (let dc = -1; dc <= 1; dc++) {
      if (dr === 0 && dc === 0) continue
      const nr = (r + dr + rows) % rows
      const nc = (c + dc + cols) % cols
      count += g[nr][nc]
    }
  }
  return count
}

function step() {
  const next = createGrid()
  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      const n = countNeighbors(grid, r, c)
      if (grid[r][c]) {
        next[r][c] = n === 2 || n === 3 ? 1 : 0
      } else {
        next[r][c] = n === 3 ? 1 : 0
      }
    }
  }
  grid = next
  generation.value++
  draw()
}

function draw() {
  const ctx = canvas.value?.getContext('2d')
  if (!ctx) return

  ctx.fillStyle = '#1a1a1a'
  ctx.fillRect(0, 0, cols * cellSize, rows * cellSize)

  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      if (grid[r][c]) {
        ctx.fillStyle = '#955438'
        ctx.fillRect(c * cellSize + 1, r * cellSize + 1, cellSize - 2, cellSize - 2)
      } else {
        ctx.strokeStyle = '#2a2a2a'
        ctx.strokeRect(c * cellSize + 0.5, r * cellSize + 0.5, cellSize - 1, cellSize - 1)
      }
    }
  }
}

function toggleCell(e) {
  const rect = canvas.value.getBoundingClientRect()
  const scaleX = (cols * cellSize) / rect.width
  const scaleY = (rows * cellSize) / rect.height
  const c = Math.floor((e.clientX - rect.left) * scaleX / cellSize)
  const r = Math.floor((e.clientY - rect.top) * scaleY / cellSize)
  if (r >= 0 && r < rows && c >= 0 && c < cols) {
    grid[r][c] = grid[r][c] ? 0 : 1
    draw()
  }
}

let timer = null

function loop() {
  if (!running.value) return
  step()
  timer = setTimeout(loop, 80)
}

function start() {
  running.value = true
  loop()
}

function stop() {
  running.value = false
  if (timer) {
    clearTimeout(timer)
    timer = null
  }
}

onMounted(() => {
  randomize()
})

onUnmounted(() => {
  stop()
})
</script>
