<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

// Props 定義
const props = defineProps({
  modelValue: {
    type: Number,
    default: 0
  },
  min: {
    type: Number,
    default: 0
  },
  max: {
    type: Number,
    default: 100
  },
  size: {
    type: [Number, String],
    default: 120
  },
  primaryColor: {
    type: String,
    default: '#3b82f6'
  },
  trackColor: {
    type: String,
    default: '#CDCD9A'
  },
  strokeWidth: {
    type: Number,
    default: 5
  },
  fillColor: {
    type: String,
    default: '#00FF00'
  },
  startAngle: {
    type: Number,
    default: -90 // 預設從 12 點鐘方向開始
  }
})

// Emits 定義
const emit = defineEmits(['update:modelValue'])

// Refs
const knobRef = ref(null)
const isDragging = ref(false)

// 計算尺寸相關數值
const sizeNum = computed(() => {
  return typeof props.size === 'string' ? parseInt(props.size) : props.size
})

const center = computed(() => sizeNum.value / 2)
const radius = computed(() => (sizeNum.value - props.strokeWidth) / 2) // 留出 stroke 寬度
const circumference = computed(() => 2 * Math.PI * radius.value)

// 計算當前百分比
const percentage = computed(() => {
  const range = props.max - props.min
  if (range === 0) return 0
  return (props.modelValue - props.min) / range
})

// 計算 stroke-dashoffset 實現進度條效果
const strokeDashoffset = computed(() => {
  return circumference.value * (1 - percentage.value)
})

// 將角度轉換為數值
const angleToValue = (angle) => {
  // 將角度標準化到 0-360 範圍
  let normalizedAngle = angle - props.startAngle
  if (normalizedAngle < 0) normalizedAngle += 360
  if (normalizedAngle > 360) normalizedAngle -= 360

  // 轉換為百分比再轉換為數值
  const percent = normalizedAngle / 360
  const range = props.max - props.min
  let value = props.min + percent * range

  // 限制在 min/max 範圍內
  value = Math.max(props.min, Math.min(props.max, value))

  return Math.round(value)
}

// 計算滑鼠相對於元件中心的角度
const getAngleFromEvent = (event) => {
  if (!knobRef.value) return 0

  const rect = knobRef.value.getBoundingClientRect()
  const centerX = rect.left + rect.width / 2
  const centerY = rect.top + rect.height / 2

  // 取得滑鼠/觸控位置
  const clientX = event.touches ? event.touches[0].clientX : event.clientX
  const clientY = event.touches ? event.touches[0].clientY : event.clientY

  // 使用 atan2 計算角度
  const deltaX = clientX - centerX
  const deltaY = clientY - centerY

  // atan2 返回 -PI 到 PI，轉換為 0-360 度
  let angle = Math.atan2(deltaY, deltaX) * (180 / Math.PI)
  angle = (angle + 360) % 360

  return angle
}

// 更新數值
const updateValue = (event) => {
  const angle = getAngleFromEvent(event)
  const newValue = angleToValue(angle)
  emit('update:modelValue', newValue)
}

// 滑鼠事件處理
const handleMouseDown = (event) => {
  event.preventDefault()
  isDragging.value = true
  updateValue(event)

  document.addEventListener('mousemove', handleMouseMove)
  document.addEventListener('mouseup', handleMouseUp)
}

const handleMouseMove = (event) => {
  if (!isDragging.value) return
  updateValue(event)
}

const handleMouseUp = () => {
  isDragging.value = false
  document.removeEventListener('mousemove', handleMouseMove)
  document.removeEventListener('mouseup', handleMouseUp)
}

// 觸控事件處理
const handleTouchStart = (event) => {
  event.preventDefault()
  isDragging.value = true
  updateValue(event)

  document.addEventListener('touchmove', handleTouchMove, { passive: false })
  document.addEventListener('touchend', handleTouchEnd)
}

const handleTouchMove = (event) => {
  if (!isDragging.value) return
  event.preventDefault()
  updateValue(event)
}

const handleTouchEnd = () => {
  isDragging.value = false
  document.removeEventListener('touchmove', handleTouchMove)
  document.removeEventListener('touchend', handleTouchEnd)
}

// 清理事件監聽
onUnmounted(() => {
  document.removeEventListener('mousemove', handleMouseMove)
  document.removeEventListener('mouseup', handleMouseUp)
  document.removeEventListener('touchmove', handleTouchMove)
  document.removeEventListener('touchend', handleTouchEnd)
})
</script>

<template>
  <div
    ref="knobRef"
    class="knob-control"
    :style="{
      width: sizeNum + 'px',
      height: sizeNum + 'px'
    }"
    @mousedown="handleMouseDown"
    @touchstart="handleTouchStart"
  >
    <!-- SVG 環形進度條 -->
    <svg
      :width="sizeNum"
      :height="sizeNum"
      class="knob-svg"
    >
      <!-- 中心背景填充 -->
      <circle
        :cx="center"
        :cy="center"
        :r="radius - strokeWidth / 2"
        :fill="fillColor"
      />

      <!-- 背景軌道 -->
      <circle
        :cx="center"
        :cy="center"
        :r="radius"
        class="knob-track"
        fill="none"
        :stroke-width="strokeWidth"
        :stroke="trackColor"
      />

      <!-- 進度條 -->
      <circle
        :cx="center"
        :cy="center"
        :r="radius"
        class="knob-progress"
        fill="none"
        :stroke-width="strokeWidth"
        :stroke="primaryColor"
        stroke-linecap="round"
        :stroke-dasharray="circumference"
        :stroke-dashoffset="strokeDashoffset"
        :style="{
          transform: `rotate(${startAngle}deg)`,
          transformOrigin: 'center'
        }"
      />
    </svg>

    <!-- 中心數值顯示 -->
    <div class="knob-value">
      {{ modelValue }}
    </div>
  </div>
</template>

<style scoped>
.knob-control {
  position: relative;
  cursor: grab;
  user-select: none;
  touch-action: none;
}

.knob-control:active {
  cursor: grabbing;
}

.knob-svg {
  display: block;
}

.knob-track {
  /* stroke 由 trackColor prop 控制 */
}

.knob-progress {
  transition: stroke-dashoffset 0.1s ease-out;
}

.knob-value {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 1.5rem;
  font-weight: 600;
  color: var(--color-text, #333);
  pointer-events: none;
}
</style>
