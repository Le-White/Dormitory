<template>
  <el-card class="weather-card">
    <div class="weather-container">
      <div class="weather-header">
        <div class="location-info">
          <el-icon><Location /></el-icon>
          <span class="location">{{ location }}</span>
        </div>
        <span class="date">{{ currentDate }}</span>
      </div>
      <div class="weather-info">
        <div class="weather-main">
          <div class="weather-icon">
            <el-icon :size="48"><Sunny /></el-icon>
          </div>
          <div class="temperature">{{ temperature }}°C</div>
        </div>
        <div class="weather-desc">{{ weatherDesc }}</div>
        <div class="other-info">
          <div class="info-item">
            <el-icon><Watermelon /></el-icon>
            <span>湿度: {{ humidity }}%</span>
          </div>
          <div class="info-item">
            <el-icon><WindPower /></el-icon>
            <span>风速: {{ windSpeed }}km/h</span>
          </div>
        </div>
      </div>
    </div>
  </el-card>
</template>

<script>
import { Location, Sunny, Watermelon, WindPower } from '@element-plus/icons-vue'

export default {
  name: 'Weather',
  components: {
    Location,
    Sunny,
    Watermelon,
    WindPower
  },
  data() {
    return {
      location: '陕西省',
      temperature: '25',
      weatherDesc: '晴',
      humidity: '45',
      windSpeed: '3.5',
      currentDate: ''
    }
  },
  created() {
    this.updateDate();
    setInterval(this.updateDate, 60000);
  },
  methods: {
    updateDate() {
      const now = new Date();
      const options = { 
        weekday: 'long', 
        year: 'numeric', 
        month: 'long', 
        day: 'numeric',
        hour: '2-digit',
        minute: '2-digit'
      };
      this.currentDate = now.toLocaleDateString('zh-CN', options);
    }
  }
}
</script>

<style scoped>
.weather-card {
  width: 100%;
  background: linear-gradient(120deg, #6CA6CD, #4A708B);
  color: white;
  border: none;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  backdrop-filter: blur(10px);
}

:deep(.el-card__body) {
  padding: 0;
}

.weather-container {
  padding: 24px;
}

.weather-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24px;
}

.location-info {
  display: flex;
  align-items: center;
  gap: 8px;
}

.location {
  font-size: 20px;
  font-weight: bold;
}

.date {
  font-size: 14px;
  opacity: 0.8;
}

.weather-info {
  text-align: center;
}

.weather-main {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 20px;
  margin-bottom: 16px;
}

.weather-icon {
  animation: float 3s ease-in-out infinite;
}

.temperature {
  font-size: 48px;
  font-weight: bold;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
}

.weather-desc {
  font-size: 24px;
  margin-bottom: 20px;
  font-weight: 500;
}

.other-info {
  display: flex;
  justify-content: space-around;
  padding: 16px 0;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 8px;
}

.info-item {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 14px;
}

.weather-card {
  transition: all 0.3s ease;
}

.weather-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
}

@keyframes float {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}

/* 添加毛玻璃效果 */
:deep(.el-card__body) {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-radius: 8px;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .weather-container {
    padding: 16px;
  }

  .temperature {
    font-size: 36px;
  }

  .weather-desc {
    font-size: 20px;
  }

  .other-info {
    flex-direction: column;
    gap: 12px;
    align-items: center;
  }
}
</style>