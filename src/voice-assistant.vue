<template>
  <div class="voice-assistant">
    <!-- 顶部导航 -->
    <header class="header">
      <div class="logo">
        <div class="logo-icon">🐺</div>
        <span class="logo-text">HuskyAI 语音助手</span>
      </div>
    </header>

    <!-- 中间动态圆形 -->
    <main class="main-content">
      <div class="voice-visualizer">
        <!-- 外层波纹圆环 -->
        <div class="pulse-ring pulse-ring-1"></div>
        <div class="pulse-ring pulse-ring-2"></div>
        <div class="pulse-ring pulse-ring-3"></div>

        <!-- 中心圆 -->
        <div class="center-circle">
          <div class="inner-glow"></div>
          <svg class="mic-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M12 1a3 3 0 0 0-3 3v8a3 3 0 0 0 6 0V4a3 3 0 0 0-3-3z"></path>
            <path d="M19 10v2a7 7 0 0 1-14 0v-2"></path>
            <line x1="12" y1="19" x2="12" y2="23"></line>
            <line x1="8" y1="23" x2="16" y2="23"></line>
          </svg>
        </div>
      </div>

      <!-- 状态文字 -->
      <div class="status-text">
        {{ statusText }}
      </div>
    </main>

    <!-- 底部退出按钮 -->
    <footer class="footer">
      <button class="exit-button" @click="handleExit">
        <svg class="exit-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"></path>
          <polyline points="16 17 21 12 16 7"></polyline>
          <line x1="21" y1="12" x2="9" y2="12"></line>
        </svg>
        <span>退出语音模式</span>
      </button>
    </footer>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const statusText = ref('正在聆听...')
let animationFrame

// 模拟状态变化
const statusMessages = ['正在聆听...', '正在思考...', '正在回答...']
let currentIndex = 0

onMounted(() => {
  // 可以添加状态切换逻辑
  const interval = setInterval(() => {
    currentIndex = (currentIndex + 1) % statusMessages.length
    statusText.value = statusMessages[currentIndex]
  }, 3000)

  onUnmounted(() => {
    clearInterval(interval)
  })
})

import { useRouter } from 'vue-router'

const router = useRouter()

const handleExit = () => {
  router.push('/')
}
</script>

<style scoped>
.voice-assistant {
  min-height: 100vh;
  background: linear-gradient(135deg, #0a0e27 0%, #1a1d3a 100%);
  display: flex;
  flex-direction: column;
  color: white;
  font-family: system-ui, -apple-system, sans-serif;
}

/* 顶部导航 */
.header {
  padding: 1.5rem 2rem;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.logo {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.logo-icon {
  font-size: 2rem;
  animation: float 3s ease-in-out infinite;
}

.logo-text {
  font-size: 1.25rem;
  font-weight: 600;
  letter-spacing: 0.5px;
}

/* 中间内容区 */
.main-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 2rem;
  gap: 3rem;
}

/* 语音可视化器 */
.voice-visualizer {
  position: relative;
  width: 280px;
  height: 280px;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* 脉冲波纹效果 */
.pulse-ring {
  position: absolute;
  border-radius: 50%;
  border: 2px solid rgba(59, 130, 246, 0.4);
  animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}

.pulse-ring-1 {
  width: 100%;
  height: 100%;
  animation-delay: 0s;
}

.pulse-ring-2 {
  width: 80%;
  height: 80%;
  animation-delay: 0.3s;
}

.pulse-ring-3 {
  width: 60%;
  height: 60%;
  animation-delay: 0.6s;
}

@keyframes pulse {
  0%, 100% {
    transform: scale(0.9);
    opacity: 1;
  }
  50% {
    transform: scale(1.1);
    opacity: 0.3;
  }
}

/* 中心圆 */
.center-circle {
  position: relative;
  width: 160px;
  height: 160px;
  border-radius: 50%;
  background: linear-gradient(135deg, #3b82f6 0%, #8b5cf6 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 0 60px rgba(59, 130, 246, 0.6),
  0 0 100px rgba(139, 92, 246, 0.4);
  animation: glow 2s ease-in-out infinite alternate;
  cursor: pointer;
  transition: transform 0.3s ease;
}

.center-circle:hover {
  transform: scale(1.05);
}

.inner-glow {
  position: absolute;
  width: 80%;
  height: 80%;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(255, 255, 255, 0.3) 0%, transparent 70%);
  animation: innerPulse 2s ease-in-out infinite;
}

@keyframes glow {
  from {
    box-shadow: 0 0 40px rgba(59, 130, 246, 0.5),
    0 0 80px rgba(139, 92, 246, 0.3);
  }
  to {
    box-shadow: 0 0 80px rgba(59, 130, 246, 0.8),
    0 0 120px rgba(139, 92, 246, 0.5);
  }
}

@keyframes innerPulse {
  0%, 100% {
    opacity: 0.5;
    transform: scale(0.9);
  }
  50% {
    opacity: 0.8;
    transform: scale(1.1);
  }
}

/* 麦克风图标 */
.mic-icon {
  width: 48px;
  height: 48px;
  color: white;
  z-index: 1;
  filter: drop-shadow(0 2px 8px rgba(0, 0, 0, 0.3));
}

/* 状态文字 */
.status-text {
  font-size: 1.5rem;
  font-weight: 500;
  color: rgba(255, 255, 255, 0.9);
  animation: fadeInOut 2s ease-in-out infinite;
}

@keyframes fadeInOut {
  0%, 100% {
    opacity: 0.6;
  }
  50% {
    opacity: 1;
  }
}

@keyframes float {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-8px);
  }
}

/* 底部 */
.footer {
  padding: 2rem;
  display: flex;
  justify-content: center;
}

/* 退出按钮 */
.exit-button {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 1rem 2rem;
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 12px;
  color: white;
  font-size: 1rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.3s ease;
  backdrop-filter: blur(10px);
}

.exit-button:hover {
  background: rgba(255, 255, 255, 0.15);
  border-color: rgba(255, 255, 255, 0.3);
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
}

.exit-button:active {
  transform: translateY(0);
}

.exit-icon {
  width: 20px;
  height: 20px;
}

/* 响应式设计 */
@media (max-width: 640px) {
  .header {
    padding: 1rem 1.5rem;
  }

  .logo-text {
    font-size: 1.1rem;
  }

  .voice-visualizer {
    width: 240px;
    height: 240px;
  }

  .center-circle {
    width: 140px;
    height: 140px;
  }

  .mic-icon {
    width: 40px;
    height: 40px;
  }

  .status-text {
    font-size: 1.25rem;
  }

  .exit-button {
    padding: 0.875rem 1.5rem;
    font-size: 0.95rem;
  }
}
</style>