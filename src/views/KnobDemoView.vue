<script setup>
import { ref } from 'vue'
import KnobControl from '@/components/KnobControl.vue'

// 示范用的数值
const value1 = ref(50)
const value2 = ref(25)
const value3 = ref(75)
</script>

<template>
  <div class="knob-demo">
    <h1>Knob 元件示范</h1>

    <div class="demo-section">
      <h2>基本用法</h2>
      <div class="demo-row">
        <div class="demo-item">
          <KnobControl v-model="value1" />
          <p>当前数值：{{ value1 }}</p>
        </div>
      </div>
    </div>

    <div class="demo-section">
      <h2>自定义属性</h2>
      <div class="demo-row">
        <div class="demo-item">
          <KnobControl
            v-model="value2"
            :size="100"
            primary-color="#ef4444"
            :min="0"
            :max="50"
          />
          <p>小尺寸 / 绿色 / 0-50</p>
          <p>数值：{{ value2 }}</p>
        </div>

        <div class="demo-item">
          <KnobControl
            v-model="value3"
            :size="160"
            primary-color="#ef4444"
            :min="0"
            :max="180"
            :start-angle="150"
          />
          <p>大尺寸 / 橙色 / 3点钟起始</p>
          <p>数值：{{ value3 }}</p>
        </div>
      </div>
    </div>

    <div class="demo-section">
      <h2>使用说明</h2>
      <ul>
        <li>点击并拖拽旋钮来调整数值</li>
        <li>支援滑鼠和触控操作</li>
        <li>数值会即时更新并通知父组件</li>
      </ul>
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
</style>
