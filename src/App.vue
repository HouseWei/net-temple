<script setup lang="ts">
import { ref, computed, watch } from 'vue'

const count = ref(0)
const dialogVisible = ref(false)

// 限制计数范围在0-99
const incrementCount = () => {
  if (count.value < 99) {
    count.value++
  }
}

const closeDialog = ()=>{
  dialogVisible.value = false
  count.value = 0
}

// 监听计数变化，当达到99时显示对话框
watch(count, (newValue) => {
  if (newValue === 99) {
    dialogVisible.value = true
  }
})

// 计算颜色渐变
const textColor = computed(() => {
  const ratio = count.value / 99 // 0到1之间的比例
  const r = Math.round(0 + (255 - 0) * ratio) // black到gold的红色分量
  const g = Math.round(0 + (215 - 0) * ratio) // black到gold的绿色分量
  const b = Math.round(0 + (0 - 0) * ratio)   // black到gold的蓝色分量
  return `rgb(${r}, ${g}, ${b})`
})

// 检测是否所有div都变成金色
const isAllGold = computed(() => {
  return count.value >= 99
})

// BigBuddha的动态样式
const bigBuddhaStyle = computed(() => {
  if (isAllGold.value) {
    return {
      color: 'gold',
      fontSize: '3rem',
      transform: 'scale(1.2)',
      transition: 'all 0.5s ease',
      animation: 'pulse 1s infinite'
    }
  }
  return {}
})
</script>

<template>
  <div class="common-layout">
    <el-container>
      <el-header>
        <el-row :gutter="20">
              <el-col :span="8">
                <div></div>
              </el-col>
              <el-col :span="8">
                <div class="BigBuddha" :style="bigBuddhaStyle">Big Buddha</div>
              </el-col>
              <el-col :span="8">
                <div></div>
              </el-col>
            </el-row>
      </el-header>
      <el-main>
        <el-row :gutter="20">
          <el-col :span="8">
            <el-row :gutter="20">
              <el-col :span="12">
                <div class="Buddha" :style="{ color: textColor }">Buddha</div>
                <div class="Buddha" :style="{ color: textColor }">Buddha</div>
                <div class="Buddha" :style="{ color: textColor }">Buddha</div>
                <div class="Buddha" :style="{ color: textColor }">Buddha</div>
              </el-col>
              <el-col :span="12">
                <div class="Buddha" :style="{ color: textColor }">Buddha</div>
                <div class="Buddha" :style="{ color: textColor }">Buddha</div>
                <div class="Buddha" :style="{ color: textColor }">Buddha</div>
                <div class="Buddha" :style="{ color: textColor }">Buddha</div>
              </el-col>
            </el-row>
          </el-col>
          <el-col :span="8">
            <div class="count" :style="{ color: textColor }">{{ count }}</div>
          </el-col>
          <el-col :span="8">
            <el-row :gutter="20">
              <el-col :span="12">
                <div class="Buddha" :style="{ color: textColor }">Buddha</div>
                <div class="Buddha" :style="{ color: textColor }">Buddha</div>
                <div class="Buddha" :style="{ color: textColor }">Buddha</div>
                <div class="Buddha" :style="{ color: textColor }">Buddha</div>
              </el-col>
              <el-col :span="12">
                <div class="Buddha" :style="{ color: textColor }">Buddha</div>
                <div class="Buddha" :style="{ color: textColor }">Buddha</div>
                <div class="Buddha" :style="{ color: textColor }">Buddha</div>
                <div class="Buddha" :style="{ color: textColor }">Buddha</div>
              </el-col>
            </el-row>
          </el-col>
        </el-row>
      </el-main>
      <el-footer>
        <el-button @click="incrementCount">点击</el-button>
      </el-footer>
    </el-container>
  </div>
  
  <!-- 恭喜对话框 -->
  <el-dialog
    v-model="dialogVisible"
    title="🎉 恭喜"
    width="30%"
    :show-close="false"
    :close-on-click-modal="false"
    :close-on-press-escape="false"
  >
    <div class="congratulation-content">
      <h2 style="color: gold; text-align: center; margin-bottom: 20px;">
        恭喜你修炼成佛！ 🙏
      </h2>
      <p style="text-align: center; font-size: 1.2rem; color: #666;">
        经过九九八十一难的修炼，你终于功德圆满！
      </p>
    </div>
    <template #footer>
      <span class="dialog-footer">
        <el-button type="primary" @click="closeDialog">
          阿弥陀佛 🙏
        </el-button>
      </span>
    </template>
  </el-dialog>
</template>


<style scoped>
.common-layout{
  width: 100%;
  height: 100%;
  .bigBuddha{
    width: 50%;
    height: 20%;
    background-color: azure;
    img{
      visibility: hidden;
    }
  }
}
.BigBuddha{
  font-size: 2rem;
  font-weight: bold;
  color: black;
  transition: all 0.5s ease;
  transform-origin: center;
}
.Buddha{
  margin: 5rem;
  color: black;
}
.count{
  width: 100%;
  height: 100%;
  font-size: 2rem;
  font-weight: bold;
  display: flex;
  justify-content: center; 
  align-items: center;  
  flex-direction: column;
  transition: color 0.3s ease;
}

@keyframes pulse {
  0% {
    transform: scale(1.2);
  }
  50% {
    transform: scale(1.3);
  }
  100% {
    transform: scale(1.2);
  }
}

.congratulation-content {
  text-align: center;
  padding: 20px 0;
}

.dialog-footer {
  text-align: center;
  width: 100%;
}
</style>
