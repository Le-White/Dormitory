<template>
    <el-card style="margin: 15px; min-height: calc(100vh - 80px)">
        <!--    头部数据-->
        <div>
            <el-row :gutter="20" class="topInfo">
                <el-col :span="6">
                    <div id="stuNumDiv" class="el-colDiv">
                        <div id="ssv1-main-text" class="nowDiv">实时</div>
                        <span class="title">学生统计</span><br/>
                        <span class="digital">{{ this.studentNum }}</span><br/>
                        <span class="last-span">当前分类总记录数</span>
                    </div>
                </el-col>
                <el-col :span="6">
                    <div id="haveRoomDiv" class="el-colDiv">
                        <div id="ssv2-main-text" class="nowDiv">实时</div>
                        <span class="title">住宿人数</span><br/>
                        <span class="digital">{{ this.haveRoomStudentNum }}</span><br/>
                        <span class="last-span">当前分类总记录数</span>
                    </div>
                </el-col>
                <el-col :span="6">
                    <div id="repairNum" class="el-colDiv">
                        <div id="ssv3-main-text" class="nowDiv">实时</div>
                        <span class="title">报修统计</span><br/>
                        <span class="digital">{{ this.repairOrderNum }}</span><br/>
                        <span class="last-span">当前分类总记录数</span>
                    </div>
                </el-col>
                <el-col :span="6">
                    <div id="emptyRoom" class="el-colDiv">
                        <div id="ssv4-main-text" class="nowDiv">实时</div>
                        <span class="title">空宿舍统计</span><br/>
                        <span class="digital">{{ this.noFullRoomNum }}</span><br/>
                        <span class="last-span">当前分类总记录数</span>
                    </div>
                </el-col>
            </el-row>
        </div>
        <!-- 下部内容 -->
        <div class="bottom-content">
            <!--   左侧 宿舍通告-->
            <div class="notice-section">
                <span class="section-title">宿舍通告</span>
                <el-timeline>
                    <el-timeline-item 
                        v-for="(activity, index) in activities.slice(0, 8)" 
                        :key="index"
                        :timestamp="activity.releaseTime"
                        class="timeline-item"
                        @click="showNoticeDetail(activity)"
                    >
                        <div class="notice-item">
                            <span style="font-size: 15px">{{ activity.title }}</span>
                        </div>
                    </el-timeline-item>
                </el-timeline>

                <!-- 添加弹出层 -->
                <el-dialog
                    v-model="dialogVisible"
                    :title="currentNotice.title"
                    width="500px"
                    class="notice-dialog"
                    :close-on-click-modal="true"
                    :show-close="true"
                >
                    <div class="notice-header">
                        <el-tag size="small" effect="plain" type="primary">宿舍公告</el-tag>
                        <span class="notice-time">{{ currentNotice.releaseTime }}</span>
                    </div>
                    
                    <div class="notice-content" v-html="currentNotice.content"></div>
                    
                    <div class="notice-footer">
                        <el-button plain @click="dialogVisible = false">关闭</el-button>
                    </div>
                </el-dialog>
            </div>
            <!--   中部-->
            <div class="chart-section">
                <span class="section-title">宿舍学生人数分布</span>
                <home_echarts/>
            </div>
            <!--  右侧-->
            <div class="right-section">
                <!--   天气组件-->
                <weather class="weather-widget"/>
                <!--    日历组件-->
                <el-calendar v-model="value" class="custom-calendar">
                </el-calendar>
            </div>
        </div>
    </el-card>
</template>

<script src="@/assets/js/Home.js"></script>

<style scoped>
@import '../assets/css/Home.css';

.timeline-item {
    cursor: pointer;
    transition: all 0.3s ease;
}

.timeline-item:hover {
    transform: translateX(5px);
}

.notice-item {
    padding: 8px;
    border-radius: 4px;
    transition: all 0.3s ease;
}

.notice-item:hover {
    background-color: #f5f7fa;
}

.notice-content {
    padding: 20px;
    line-height: 1.6;
    color: #606266;
    max-height: 400px;
    overflow-y: auto;
}

.notice-footer {
    margin-top: 20px;
    padding-top: 10px;
    border-top: 1px solid #ebeef5;
    text-align: right;
}

.notice-time {
    color: #909399;
    font-size: 14px;
}

:deep(.el-dialog) {
    border-radius: 8px;
    overflow: hidden;
}

:deep(.el-dialog__header) {
    padding: 20px;
    margin: 0;
    background-color: #f5f7fa;
    border-bottom: 1px solid #ebeef5;
}

:deep(.el-dialog__title) {
    font-size: 18px;
    font-weight: 600;
    color: #303133;
}

:deep(.el-dialog__body) {
    padding: 0;
}

:deep(.el-timeline-item__node) {
    background-color: #409EFF;
}

:deep(.el-timeline-item__tail) {
    border-left-color: #e4e7ed;
}

:deep(.el-timeline-item:hover .el-timeline-item__node) {
    transform: scale(1.2);
    transition: all 0.3s ease;
}

.bottom-content {
    display: flex;
    margin-top: 40px;
    gap: 30px;
    align-items: flex-start;
}

.notice-section {
    flex: 1;
    min-width: 300px;
}

.chart-section {
    flex: 1.5;
    min-width: 400px;
    height: 588px;
}

.right-section {
    flex: 1;
    min-width: 300px;
    display: flex;
    flex-direction: column;
    gap: 20px;
}

.section-title {
    font-size: 22px;
    display: block;
    margin-bottom: 30px;
    margin-left: 10px;
    color: #303133;
    font-weight: 600;
}

.weather-widget {
    width: 100%;
}


/* 响应式设计 */
@media (max-width: 1400px) {
    .bottom-content {
        flex-direction: column;
        align-items: center;
    }

    .notice-section,
    .chart-section,
    .right-section {
        width: 100%;
        max-width: 800px;
    }
}

/* 通告弹窗样式 */
.notice-dialog {
    border-radius: 8px;
    overflow: hidden;
}

:deep(.el-dialog__header) {
    margin: 0;
    padding: 20px;
    background-color: #f5f7fa;
    border-bottom: 1px solid #e4e7ed;
}

:deep(.el-dialog__title) {
    font-size: 18px;
    font-weight: 600;
    color: #303133;
}

:deep(.el-dialog__headerbtn) {
    top: 20px;
}

:deep(.el-dialog__body) {
    padding: 0;
}

.notice-header {
    padding: 16px 20px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    border-bottom: 1px solid #f0f0f0;
}

.notice-time {
    color: #909399;
    font-size: 14px;
}

.notice-content {
    padding: 20px;
    min-height: 100px;
    line-height: 1.8;
    color: #606266;
    font-size: 15px;
    white-space: pre-wrap;
    word-break: break-all;
}

.notice-footer {
    padding: 16px 20px;
    text-align: right;
    background-color: #f5f7fa;
    border-top: 1px solid #e4e7ed;
}

/* 通告列表项样式 */
.timeline-item {
    cursor: pointer;
    transition: all 0.3s ease;
}

.timeline-item:hover {
    transform: translateX(5px);
}

:deep(.el-timeline-item__node) {
    background-color: #409EFF;
    transition: all 0.3s ease;
}

:deep(.el-timeline-item__tail) {
    border-left-color: #e4e7ed;
}

:deep(.el-timeline-item:hover .el-timeline-item__node) {
    transform: scale(1.2);
}

.notice-item {
    padding: 8px 12px;
    border-radius: 4px;
    transition: all 0.3s ease;
}

.notice-item:hover {
    background-color: #f5f7fa;
}

/* 滚动条美化 */
.notice-content::-webkit-scrollbar {
    width: 6px;
}

.notice-content::-webkit-scrollbar-thumb {
    background-color: #dcdfe6;
    border-radius: 3px;
}

.notice-content::-webkit-scrollbar-track {
    background-color: #f5f7fa;
}

/* 响应式设计 */
@media (max-width: 768px) {
    :deep(.el-dialog) {
        width: 90% !important;
        margin: 0 auto;
    }

    .notice-content {
        padding: 15px;
        font-size: 14px;
    }
}

.custom-calendar {
  transform: scale(0.9); /* 缩小到90% */
  transform-origin: top left;
  margin: -10px; /* 调整边距以补偿缩放 */
}


</style>