<script setup>
import { ref } from 'vue'
import KnobControl from '@/components/KnobControl.vue'

// 示範用的數值
const value1 = ref(50)
const value2 = ref(25)
const value3 = ref(75)
const value4 = ref(60)
</script>

<template>
  <div class="knob-demo">
    <h1>Knob 元件示範</h1>

    <!-- <div class="demo-section">
      <h2>基本用法</h2>
      <div class="demo-row">
        <div class="demo-item">
          <KnobControl v-model="value1" />
          <p>預設樣式</p>
          <p>數值：{{ value1 }}</p>
        </div>
      </div>
    </div> -->

    <div class="demo-section">
      <h2>自定義顏色</h2>
      <div class="demo-row">
        <div class="demo-item">
          <KnobControl
            v-model="value2"
            :size="100"
            primary-color="#ef4444"
            track-color="#fecaca"
            fill-color="#2F0000"
            :min="0"
            :max="50"
          />
        </div>

        <div class="demo-item">
          <KnobControl
            v-model="value3"
            :size="120"
            primary-color="#10b981"
            track-color="#d1fae5"
            fill-color="#ecfdf5"
            :min="0"
            :max="100"
          />
        </div>

        <div class="demo-item">
          <KnobControl
            v-model="value4"
            :size="140"
            primary-color="#f59e0b"
            track-color="#fef3c7"
            fill-color="#fffbeb"
            :stroke-width="65"
            :min="0"
            :max="100"
            :start-angle="0"
          />
        </div>
      </div>
    </div>

    <div class="demo-section">
      <h2>Props 說明</h2>
      <table class="props-table">
        <thead>
          <tr>
            <th>Prop</th>
            <th>Type</th>
            <th>說明</th>
          </tr>
        </thead>
        <tbody>
          <tr><td>v-model</td><td>Number</td><td>當前數值</td></tr>
          <tr><td>min</td><td>Number</td><td>最小值</td></tr>
          <tr><td>max</td><td>Number</td><td>最大值</td></tr>
          <tr><td>size</td><td>Number/String</td><td>元件尺寸</td></tr>
          <tr><td>primary-color</td><td>String</td><td>進度條顏色</td></tr>
          <tr><td>track-color</td><td>String</td><td>背景軌道顏色</td></tr>
          <tr><td>fill-color</td><td>String</td><td>中心背景填充色</td></tr>
          <tr><td>stroke-width</td><td>Number</td><td>軌道粗細</td></tr>
          <tr><td>start-angle</td><td>Number</td><td>起始角度</td></tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
.knob-demo {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
}

h1 {
  text-align: center;
  margin-bottom: 2rem;
  color: var(--color-heading);
}

h2 {
  margin-bottom: 1rem;
  color: var(--color-heading);
  border-bottom: 1px solid var(--color-border);
  padding-bottom: 0.5rem;
}

.demo-section {
  margin-bottom: 3rem;
}

.demo-row {
  display: flex;
  flex-wrap: wrap;
  gap: 2rem;
  justify-content: center;
}

.demo-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
}

.demo-item p {
  margin: 0;
  text-align: center;
  color: var(--color-text);
}

ul {
  line-height: 1.8;
  color: var(--color-text);
}

.props-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 1rem;
}

.props-table th,
.props-table td {
  border: 1px solid var(--color-border);
  padding: 0.5rem 1rem;
  text-align: left;
}

.props-table th {
  background-color: var(--color-background-soft);
  font-weight: 600;
}

.props-table td:first-child {
  font-family: monospace;
  color: var(--color-heading);
}
</style>
