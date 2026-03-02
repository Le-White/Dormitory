<template>
  <div class="my-room">
    <el-breadcrumb separator-icon="ArrowRight" class="breadcrumb">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>我的宿舍</el-breadcrumb-item>
    </el-breadcrumb>
    
    <el-card class="room-card">
      <div class="room-container">
        <!-- 左侧信息区域 -->
        <div class="info-section">
          <!-- 房间信息 -->
          <el-descriptions 
            :column="1" 
            border 
            title="房间信息"
            class="info-block"
          >
            <el-descriptions-item>
              <template #label>
                <div class="label-content">
                  <el-icon><user /></el-icon>
                  <span>楼宇号</span>
                </div>
              </template>
              <span class="info-text">{{ room.dormBuildId }}</span>
            </el-descriptions-item>
            <el-descriptions-item>
              <template #label>
                <div class="label-content">
                  <el-icon><location /></el-icon>
                  <span>房间号</span>
                </div>
              </template>
              <span class="info-text">{{ room.dormRoomId }}</span>
            </el-descriptions-item>
            <el-descriptions-item>
              <template #label>
                <div class="label-content">
                  <el-icon><tickets /></el-icon>
                  <span>楼层</span>
                </div>
              </template>
              <span class="info-text">{{ room.floorNum }}</span>
            </el-descriptions-item>
            <el-descriptions-item>
              <template #label>
                <div class="label-content">
                  <el-icon><office-building /></el-icon>
                  <span>可住人数</span>
                </div>
              </template>
              <span class="info-text">{{ room.maxCapacity }}</span>
            </el-descriptions-item>
            <el-descriptions-item>
              <template #label>
                <div class="label-content">
                  <el-icon><iphone /></el-icon>
                  <span>已住人数</span>
                </div>
              </template>
              <span class="info-text">{{ room.currentCapacity }}</span>
            </el-descriptions-item>
          </el-descriptions>

          <!-- 床位信息 -->
          <el-descriptions 
            :column="1" 
            border 
            title="床位信息"
            class="info-block"
          >
            <el-descriptions-item>
              <template #label>
                <div class="label-content">
                  <el-icon><user /></el-icon>
                  <span>一号床位</span>
                </div>
              </template>
              <el-tag
                v-if="room.firstBed"
                :type="name === room.firstBed ? 'primary' : 'info'"
                class="bed-tag"
                @click="handleBedClick(room.firstBed)"
              >
                {{ room.firstBed }}
              </el-tag>
            </el-descriptions-item>
            <el-descriptions-item>
              <template #label>
                <div class="label-content">
                  <el-icon><location /></el-icon>
                  <span>二号床位</span>
                </div>
              </template>
              <el-tag
                v-if="room.secondBed"
                :type="name === room.secondBed ? 'primary' : 'info'"
                class="bed-tag"
                @click="handleBedClick(room.secondBed)"
              >
                {{ room.secondBed }}
              </el-tag>
            </el-descriptions-item>
            <el-descriptions-item>
              <template #label>
                <div class="label-content">
                  <el-icon><tickets /></el-icon>
                  <span>三号床位</span>
                </div>
              </template>
              <el-tag
                v-if="room.thirdBed"
                :type="name === room.thirdBed ? 'primary' : 'info'"
                class="bed-tag"
                @click="handleBedClick(room.thirdBed)"
              >
                {{ room.thirdBed }}
              </el-tag>
            </el-descriptions-item>
            <el-descriptions-item>
              <template #label>
                <div class="label-content">
                  <el-icon><office-building /></el-icon>
                  <span>四号床位</span>
                </div>
              </template>
              <el-tag
                v-if="room.fourthBed"
                :type="name === room.fourthBed ? 'primary' : 'info'"
                class="bed-tag"
                @click="handleBedClick(room.fourthBed)"
              >
                {{ room.fourthBed }}
              </el-tag>
            </el-descriptions-item>
          </el-descriptions>
        </div>

        <!-- 右侧图片区域 -->
        <div class="image-section">
          <img src="../../public/大学校园.svg" alt="宿舍图片" class="room-image">
        </div>
      </div>
    </el-card>
  </div>
</template>

<script src="@/assets/js/MyRoomInfo.js"></script>

<style scoped>
.my-room {
  padding: 20px;
}

.breadcrumb {
  margin-bottom: 20px;
}

.room-card {
  min-height: calc(100vh - 150px);
  background: #fff;
  border-radius: 8px;
}

.room-container {
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  gap: 60px;
  padding: 20px;
  margin-top: 20px;
}

.info-section {
  flex: 0 1 500px;
  width: 500px;
  margin-left: 30px;
}

.info-block {
  margin-bottom: 30px;
  background: #fff;
  border-radius: 8px;
  overflow: hidden;
}

.info-block:last-child {
  margin-bottom: 0;
}

.label-content {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 15px;
}

.label-content .el-icon {
  font-size: 18px;
  color: #409EFF;
}

.info-text {
  font-size: 15px;
  color: #606266;
}

.bed-tag {
  font-size: 14px;
  padding: 6px 12px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.bed-tag:hover {
  transform: translateY(-2px);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.image-section {
  flex: 0 1 400px;
  width: 400px;
  margin-top: 50px;
}

.room-image {
  width: 100%;
  height: auto;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
}

.room-image:hover {
  transform: scale(1.02);
}

:deep(.el-descriptions__title) {
  font-size: 18px;
  font-weight: 600;
  color: #303133;
  margin-bottom: 20px;
}

:deep(.el-descriptions__label) {
  background-color: #f5f7fa !important;
  padding: 16px !important;
}

:deep(.el-descriptions__content) {
  padding: 16px !important;
}

:deep(.el-tag) {
  border-radius: 4px;
}

/* 响应式设计 */
@media (max-width: 1200px) {
  .room-container {
    flex-direction: column;
    padding: 20px;
  }

  .info-section {
    margin-left: 0;
  }

  .info-section, .image-section {
    width: 100%;
    max-width: 500px;
  }

  .image-section {
    margin-top: 30px;
  }
}
</style>