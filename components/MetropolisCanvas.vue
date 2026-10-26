<script setup>
import { ref, onMounted, watch } from 'vue'

const props = defineProps({
  mode: { type: String, default: 'artwork' }, // 'single', 'divide9', 'recursive', 'anatomy', 'chaos', 'shared-edge', 'artwork', 'interactive'
  level: { type: Number, default: 4 },
  seed: { type: Number, default: 4242 },
  chaos: { type: Number, default: 0 },
  showBorders: { type: Boolean, default: false },
  showFloors: { type: Boolean, default: true },
  showShapes: { type: Boolean, default: true },
  night: { type: Boolean, default: false },
  paletteIndex: { type: Number, default: 0 },
  interactive: { type: Boolean, default: false },
  width: { type: Number, default: 480 },
  height: { type: Number, default: 620 }
})

const canvas = ref(null)
const currentSeed = ref(props.seed)
const currentLevel = ref(props.level)
const currentChaos = ref(props.chaos)
const isNight = ref(props.night)
const currentPalette = ref(props.paletteIndex)
const hoveredCell = ref(-1)

const palettes = [
  { name: 'Mineral', colors: ['#ffbf00', '#fa8400', '#08a6a8', '#de7309'], hueRange: [195, 230] },
  { name: 'Stars', colors: ['#780000', '#c1121f', '#fdf0d5', '#003049', '#669bbc'], hueRange: [15, 42] },
  { name: 'Bauhaus', colors: ['#edbd46', '#e0c799', '#0d3790', '#a6865f', '#c2c2b3'], hueRange: [205, 255] },
  { name: 'Nebulae', colors: ['#471ca8', '#884ab2', '#ff930a', '#f24b04', '#d1105a'], hueRange: [295, 345] },
  { name: 'Earth', colors: ['#335c67', '#fff3b0', '#e09f3e', '#9e2a2b', '#540b0e'], hueRange: [25, 58] }
]

function createRNG(seedVal) {
  let state = (seedVal >>> 0) || 1
  return () => {
    state = (1664525 * state + 1013904223) >>> 0
    return state / 4294967296
  }
}

function hsbToRgb(h, s, b) {
  h = ((h % 360) + 360) % 360
  s = Math.max(0, Math.min(100, s)) / 100
  b = Math.max(0, Math.min(100, b)) / 100
  const k = (n) => (n + h / 60) % 6
  const f = (n) => b * (1 - s * Math.max(0, Math.min(k(n), 4 - k(n), 1)))
  return `rgb(${Math.round(f(5) * 255)}, ${Math.round(f(3) * 255)}, ${Math.round(f(1) * 255)})`
}

// 9-way Metropolis IX subdivision of a rectangle
function subdivide9(rect, level, rng, threshX = 0.14, threshY = 0.55, marginXDiv = 14, marginYDiv = 32) {
  const { x, y, w, h } = rect
  if (level <= 0 || w < 6 || h < 6) {
    return [{
      A: { x, y },
      B: { x: x + w, y },
      C: { x: x + w, y: y + h },
      D: { x, y: y + h },
      x, y, w, h,
      center: { x: x + w / 2, y: y + h / 2 },
      ratioXY: w / (h || 1),
      ratioYX: h / (w || 1)
    }]
  }

  const mx = w / marginXDiv
  const my = h / marginYDiv
  let x1, y1, x2, y2
  let tries = 0
  do {
    x1 = x + mx + rng() * Math.max(1, w - 2 * mx)
    x2 = x + mx + rng() * Math.max(1, w - 2 * mx)
    y1 = y + my + rng() * Math.max(1, h - 2 * my)
    y2 = y + my + rng() * Math.max(1, h - 2 * my)
    tries++
  } while (tries < 60 && !(Math.abs(x1 - x2) > w * threshX && Math.abs(y1 - y2) > h * threshY))

  const ix1 = Math.min(x1, x2)
  const ix2 = Math.max(x1, x2)
  const iy1 = Math.min(y1, y2)
  const iy2 = Math.max(y1, y2)

  // 9 sub-rectangles: center + 4 corners + 4 edge bridges
  const children = [
    { x: ix1, y: iy1, w: ix2 - ix1, h: iy2 - iy1, role: 'center' }, // 0: Center (newcell)
    { x: x,   y: y,   w: ix1 - x,   h: iy1 - y,   role: 'corner' }, // 1: TL corner
    { x: ix2, y: y,   w: x + w - ix2, h: iy1 - y, role: 'corner' }, // 2: TR corner
    { x: ix2, y: iy2, w: x + w - ix2, h: y + h - iy2, role: 'corner' }, // 3: BR corner
    { x: x,   y: iy2, w: ix1 - x,   h: y + h - iy2, role: 'corner' }, // 4: BL corner
    { x: ix1, y: y,   w: ix2 - ix1, h: iy1 - y,   role: 'edge' },   // 5: Top bridge
    { x: ix2, y: iy1, w: x + w - ix2, h: iy2 - iy1, role: 'edge' }, // 6: Right bridge
    { x: ix1, y: iy2, w: ix2 - ix1, h: y + h - iy2, role: 'edge' }, // 7: Bottom bridge
    { x: x,   y: iy1, w: ix1 - x,   h: iy2 - iy1, role: 'edge' }    // 8: Left bridge
  ]

  let result = []
  for (const child of children) {
    result.push(...subdivide9(child, level - 1, rng, threshX, threshY, marginXDiv, marginYDiv))
  }
  return result
}

function mapRange(val, inMin, inMax, outMin, outMax) {
  return outMin + ((val - inMin) / (inMax - inMin || 1)) * (outMax - outMin)
}

function render() {
  const c = canvas.value
  if (!c) return
  const ctx = c.getContext('2d')
  const W = props.width
  const H = props.height
  c.width = W
  c.height = H

  const rng = createRNG(currentSeed.value)
  const pal = palettes[currentPalette.value % palettes.length]
  const [hStart, hEnd] = pal.hueRange

  ctx.clearRect(0, 0, W, H)
  ctx.fillStyle = '#0e0f11'
  ctx.fillRect(0, 0, W, H)

  const pad = Math.round(W * 0.02)
  const rootRect = { x: pad, y: pad, w: W - pad * 2, h: H - pad * 2 }

  if (props.mode === 'divide9') {
    // Architectural diagram of 1-level 9-cell division
    const cells = subdivide9(rootRect, 1, rng, 0.25, 0.25, 4.5, 4.5)
    cells.forEach((cell, idx) => {
      const isCenter = idx === 0
      const isCorner = idx >= 1 && idx <= 4
      ctx.fillStyle = isCenter
        ? 'rgba(229, 188, 130, 0.22)'
        : isCorner
          ? 'rgba(255, 255, 255, 0.04)'
          : 'rgba(229, 188, 130, 0.08)'
      ctx.fillRect(cell.x, cell.y, cell.w, cell.h)
      ctx.strokeStyle = isCenter ? '#e5bc82' : 'rgba(238, 234, 227, 0.35)'
      ctx.lineWidth = isCenter ? 2 : 1
      ctx.strokeRect(cell.x, cell.y, cell.w, cell.h)

      // Label coordinates
      ctx.fillStyle = isCenter ? '#e5bc82' : 'rgba(238, 234, 227, 0.55)'
      ctx.font = '500 11px monospace'
      ctx.textAlign = 'center'
      ctx.fillText(isCenter ? 'MERKEZ [0]' : `[${idx}]`, cell.center.x, cell.center.y + 4)
    })

    // Draw P1 and P2 points on center cell
    const center = cells[0]
    for (const [pt, label] of [[center.A, 'P1'], [center.C, 'P2']]) {
      ctx.beginPath()
      ctx.arc(pt.x, pt.y, 4.5, 0, Math.PI * 2)
      ctx.fillStyle = '#ff5e3a'
      ctx.fill()
      ctx.fillStyle = '#fff'
      ctx.font = 'bold 11px monospace'
      ctx.fillText(label, pt.x + (label === 'P1' ? -14 : 14), pt.y + (label === 'P1' ? -8 : 14))
    }
    return
  }

  // Generate cells
  const cells = subdivide9(rootRect, currentLevel.value, rng)
  const totalArea = (W - pad * 2) * (H - pad * 2)

  // Apply chaos (addChaos from decorator_backup.js)
  const chaosAmt = currentChaos.value
  if (chaosAmt > 0) {
    cells.forEach(cell => {
      const jitter = () => (rng() * 2 - 1) * chaosAmt * (W / 500)
      cell.A = { x: cell.A.x + jitter(), y: cell.A.y + jitter() }
      cell.B = { x: cell.B.x + jitter(), y: cell.B.y + jitter() }
      cell.C = { x: cell.C.x + jitter(), y: cell.C.y + jitter() }
      cell.D = { x: cell.D.x + jitter(), y: cell.D.y + jitter() }
    })
  }

  // Color and paint cells (fillColorGeneric + #drawCells)
  const sky = isNight.value ? 0.28 : 0.45
  const light = isNight.value ? 18 : 94
  const dark = isNight.value ? 88 : 14
  const satMin = isNight.value ? 75 : 15
  const satMax = isNight.value ? 20 : 88

  cells.forEach((cell, idx) => {
    const hue = mapRange(cell.center.x, 0, W, hStart, hEnd)
    const sat = mapRange(cell.center.y, 0, H, satMin, satMax)
    const bri = mapRange(cell.center.y, H * sky, H, light, dark)
    cell.hsb = { h: hue, s: sat, b: bri }
    cell.fill = hsbToRgb(hue, sat, bri)

    const m = 0.45
    ctx.fillStyle = cell.fill
    ctx.beginPath()
    ctx.moveTo(cell.A.x - m, cell.A.y - m)
    ctx.lineTo(cell.B.x + m, cell.B.y - m)
    ctx.lineTo(cell.C.x + m, cell.C.y + m)
    ctx.lineTo(cell.D.x - m, cell.D.y + m)
    ctx.closePath()
    ctx.fill()
  })

  // Decorator: Circles, Triangles, Horizontal Floors
  if (props.showShapes || props.mode === 'anatomy' || props.mode === 'artwork') {
    cells.forEach(cell => {
      const areaNorm = (cell.w * cell.h) / totalArea

      // 1. Square-ish cells -> Circles (fillWithCircles: 0.9 < ratio < 1.1)
      if (!cell.occupied && ((cell.ratioXY > 0.9 && cell.ratioXY < 1.1) || (cell.ratioYX > 0.9 && cell.ratioYX < 1.1))) {
        if (cell.w > 4) {
          const cHex = pal.colors[Math.floor(rng() * pal.colors.length)]
          ctx.fillStyle = rng() < 0.35 ? '#111214' : cHex
          ctx.beginPath()
          ctx.arc(cell.center.x, cell.center.y, (cell.w * 0.82) / 2, 0, Math.PI * 2)
          ctx.fill()
          cell.occupied = true
          cell.feature = 'circle'
        }
      }

      // 2. Vertical-ish cells -> Triangles (fillwithTriangles: 1 < ratio.yx < 1.5)
      if (!cell.occupied && cell.ratioYX > 1.0 && cell.ratioYX < 1.5 && areaNorm < 0.4 && cell.w > 5) {
        const c1 = pal.colors[Math.floor(rng() * pal.colors.length)]
        const c2 = pal.colors[Math.floor(rng() * pal.colors.length)]
        const tRatio = 2 + Math.floor(rng() * 6)
        const apexX = cell.A.x + cell.w / 2
        const apexY = cell.A.y + cell.h / tRatio

        ctx.fillStyle = c1
        ctx.beginPath()
        ctx.moveTo(apexX, apexY)
        ctx.lineTo(cell.C.x, cell.C.y)
        ctx.lineTo(cell.D.x, cell.D.y)
        ctx.closePath()
        ctx.fill()

        ctx.fillStyle = c2
        ctx.beginPath()
        ctx.moveTo(apexX, apexY)
        ctx.lineTo(cell.A.x, cell.A.y)
        ctx.lineTo(cell.B.x, cell.B.y)
        ctx.closePath()
        ctx.fill()

        cell.occupied = true
        cell.feature = 'triangle'
      }

      // 3. Lower city -> Horizontal Floor Lines (fillHorizontalLines: center.y > height * maxHeight)
      if (props.showFloors && !cell.occupied && cell.center.y > H * 0.44 && cell.w > 4 && cell.h > 6) {
        const bx = cell.A.x + cell.w * 0.15
        const bw = cell.w * 0.70
        const towerFill = hsbToRgb(cell.hsb.h - 8, Math.min(100, cell.hsb.s + 10), Math.max(8, cell.hsb.b - 12))
        ctx.fillStyle = towerFill
        ctx.fillRect(bx, cell.A.y, bw, cell.h)

        const floorCount = Math.max(2, Math.min(18, Math.floor(cell.ratioYX * 2.5)))
        const step = cell.h / floorCount
        const lineCol = hsbToRgb(cell.hsb.h, Math.max(10, cell.hsb.s - 15), Math.min(100, cell.hsb.b + 42))

        ctx.strokeStyle = lineCol
        ctx.lineWidth = Math.max(0.5, 0.75 * (W / 600))
        for (let yPos = cell.A.y + step * 0.5; yPos < cell.C.y - 1; yPos += step) {
          ctx.beginPath()
          ctx.moveTo(bx, yPos)
          ctx.lineTo(bx + bw, yPos)
          ctx.stroke()

          // Tiny window dots at edges
          ctx.fillStyle = '#fff8e7'
          ctx.fillRect(bx - 0.6, yPos - 0.6, 1.3, 1.3)
          ctx.fillRect(bx + bw - 0.6, yPos - 0.6, 1.3, 1.3)
        }
        cell.occupied = true
        cell.feature = 'floors'
      }
    })
  }

  // Optional Cell Borders (displayDebugView)
  if (props.showBorders || props.mode === 'shared-edge') {
    ctx.strokeStyle = props.mode === 'shared-edge' ? 'rgba(229, 188, 130, 0.55)' : 'rgba(255, 220, 120, 0.28)'
    ctx.lineWidth = props.mode === 'shared-edge' ? 0.9 : 0.4
    cells.forEach(cell => {
      ctx.beginPath()
      ctx.moveTo(cell.A.x, cell.A.y)
      ctx.lineTo(cell.B.x, cell.B.y)
      ctx.lineTo(cell.C.x, cell.C.y)
      ctx.lineTo(cell.D.x, cell.D.y)
      ctx.closePath()
      ctx.stroke()
    })
  }
}

function randomizeSeed() {
  currentSeed.value = Math.floor(Math.random() * 90000) + 1000
}

function cyclePalette() {
  currentPalette.value = (currentPalette.value + 1) % palettes.length
}

function toggleNight() {
  isNight.value = !isNight.value
}

watch([currentSeed, currentLevel, currentChaos, isNight, currentPalette, () => props.level, () => props.chaos, () => props.mode], () => {
  render()
})

onMounted(() => {
  render()
})
</script>

<template>
  <div class="metropolis-widget">
    <div class="canvas-frame">
      <canvas ref="canvas" @click="interactive && randomizeSeed()" :class="{ clickable: interactive }" />
    </div>
    <div v-if="interactive" class="controls-bar">
      <button class="ctrl-btn" @click="randomizeSeed" title="Yeni Tohum (Seed)">
        <span>TOHUM #{{ currentSeed }}</span>
      </button>
      <button class="ctrl-btn" @click="cyclePalette" title="Renk Paleti Değiştir">
        <span>{{ palettes[currentPalette % palettes.length].name.toUpperCase() }}</span>
      </button>
      <button class="ctrl-btn" @click="toggleNight" title="Gündüz / Gece Döngüsü">
        <span>{{ isNight ? 'GECE' : 'GÜNDÜZ' }}</span>
      </button>
      <div class="slider-group" v-if="mode === 'chaos' || mode === 'interactive'">
        <label>KAOS</label>
        <input type="range" min="0" max="14" step="0.5" v-model.number="currentChaos" />
      </div>
      <div class="slider-group" v-if="mode === 'recursive' || mode === 'interactive'">
        <label>DERİNLİK ({{ currentLevel }})</label>
        <input type="range" min="1" max="4" step="1" v-model.number="currentLevel" />
      </div>
    </div>
  </div>
</template>

<style scoped>
.metropolis-widget {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}
.canvas-frame {
  position: relative;
  border: 1px solid rgba(229, 188, 130, 0.25);
  background: #0b0c0e;
  padding: 6px;
  box-shadow: 0 22px 55px rgba(0, 0, 0, 0.75);
}
canvas {
  display: block;
  max-width: 100%;
  height: auto;
}
canvas.clickable {
  cursor: pointer;
}
.controls-bar {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-family: monospace;
  font-size: 11px;
}
.ctrl-btn {
  background: rgba(229, 188, 130, 0.08);
  border: 1px solid rgba(229, 188, 130, 0.35);
  color: #e5bc82;
  padding: 4px 10px;
  font-family: monospace;
  font-size: 11px;
  letter-spacing: 0.06em;
  cursor: pointer;
  transition: all 0.2s ease;
}
.ctrl-btn:hover {
  background: rgba(229, 188, 130, 0.22);
  color: #fff;
}
.slider-group {
  display: flex;
  align-items: center;
  gap: 6px;
  color: rgba(238, 234, 227, 0.7);
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(238, 234, 227, 0.15);
  padding: 3px 8px;
}
.slider-group input[type="range"] {
  width: 68px;
  accent-color: #e5bc82;
  cursor: pointer;
}
</style>
