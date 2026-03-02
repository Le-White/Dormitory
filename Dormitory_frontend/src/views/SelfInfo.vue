<template>
    <div class="self-info-container">
        <el-breadcrumb separator-icon="ArrowRight" class="breadcrumb">
            <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
            <el-breadcrumb-item>个人信息</el-breadcrumb-item>
        </el-breadcrumb>
        <el-card class="info-card">
            <div class="info-wrapper">
                <!-- 左侧头像区域 -->
                <div class="avatar-section">
                    <el-upload :on-success="uploadSuccess" :show-file-list="false"
                               action="http://localhost:9090/files/upload/"
                               class="avatar-uploader"
                    >
                        <div class="avatar-container">
                            <el-avatar v-if="!image" :size="120" icon="UserFilled" class="avatar" />
                            <img v-else :src="'data:image;base64,' + image" class="avatar-image" />
                            <div class="upload-overlay">
                                <el-icon><upload-filled /></el-icon>
                                <span>更换头像</span>
                            </div>
                        </div>
                    </el-upload>
                    <h2 class="username">{{ username }}</h2>
                </div>
                <!-- 右侧信息区域 -->
                <div class="info-section">
                    <el-descriptions :column="1" border class="info-descriptions">
                        <el-descriptions-item>
                            <template #label>
                                <div class="label-content">
                                    <el-icon><user /></el-icon>
                                    <span>用户名</span>
                                </div>
                            </template>
                            <span class="info-text">{{ username }}</span>
                        </el-descriptions-item>
                        <el-descriptions-item>
                            <template #label>
                                <div class="label-content">
                                    <el-icon><user-filled /></el-icon>
                                    <span>姓名</span>
                                </div>
                            </template>
                            <span class="info-text">{{ name }}</span>
                        </el-descriptions-item>
                        <el-descriptions-item>
                            <template #label>
                                <div class="label-content">
                                    <el-icon><male /></el-icon>
                                    <span>性别</span>
                                </div>
                            </template>
                            <span class="info-text">{{ gender }}</span>
                        </el-descriptions-item>
                        <el-descriptions-item>
                            <template #label>
                                <div class="label-content">
                                    <el-icon><timer /></el-icon>
                                    <span>年龄</span>
                                </div>
                            </template>
                            <span class="info-text">{{ age }}</span>
                        </el-descriptions-item>
                        <el-descriptions-item>
                            <template #label>
                                <div class="label-content">
                                    <el-icon><iphone /></el-icon>
                                    <span>手机号</span>
                                </div>
                            </template>
                            <span class="info-text">{{ phoneNum }}</span>
                        </el-descriptions-item>
                        <el-descriptions-item>
                            <template #label>
                                <div class="label-content">
                                    <el-icon><message /></el-icon>
                                    <span>邮箱</span>
                                </div>
                            </template>
                            <span class="info-text">{{ email }}</span>
                        </el-descriptions-item>
                    </el-descriptions>
                    <el-button type="primary" class="edit-button" @click="Edit">
                        <el-icon><edit /></el-icon>
                        修改信息
                    </el-button>
                </div>
            </div>
        </el-card>
        <!-- 修改信息对话框 -->
        <el-dialog v-model="dialogVisible" title="修改个人信息" width="500px" @close="cancel" class="edit-dialog">
            <el-form ref="form" :model="form" :rules="rules" label-width="120px">
                <el-form-item label="账号" prop="username">
                    <el-input v-model="form.username" disabled style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="密码" prop="password">
                    <div class="password-input-group">
                        <el-input 
                            v-model="form.password" 
                            :disabled="disabled" 
                            :show-password="showpassword"
                            class="password-input"
                        ></el-input>
                        <el-tooltip content="修改密码" placement="right">
                            <el-icon 
                                class="edit-password-icon"
                                @click="EditPass"
                            >
                                <Edit />
                            </el-icon>
                        </el-tooltip>
                    </div>
                </el-form-item>
                <el-form-item :style="display" label="确认密码" prop="checkPass">
                    <el-input v-model="form.checkPass" show-password style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="姓名" prop="name">
                    <el-input v-model="form.name" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="性别" prop="gender">
                    <el-radio v-model="form.gender" label="男">男</el-radio>
                    <el-radio v-model="form.gender" label="女">女</el-radio>
                </el-form-item>
                <el-form-item label="年龄" prop="age">
                    <el-input v-model.number="form.age" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="手机号" prop="phoneNum">
                    <el-input v-model.number="form.phoneNum" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="邮箱地址" prop="email">
                    <el-input v-model="form.email" style="width: 80%"></el-input>
                </el-form-item>
            </el-form>
            <template #footer>
                <span class="dialog-footer">
                    <el-button @click="cancel">取 消</el-button>
                    <el-button type="primary" @click="save">确 定</el-button>
                </span>
            </template>
        </el-dialog>
    </div>
</template>
<script src="@/assets/js/SelfInfo.js"></script>
<style scoped>
.self-info-container {
    padding: 20px;
    min-height: 100vh;
    background-color: #f5f7fa;
}

.breadcrumb {
    margin-bottom: 20px;
}

.info-card {
    background: white;
    border-radius: 8px;
    box-shadow: 0 2px 12px 0 rgba(0,0,0,0.1);
}

.info-wrapper {
    display: flex;
    gap: 40px;
    padding: 30px;
}

.avatar-section {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 20px;
}

.avatar-container {
    position: relative;
    width: 120px;
    height: 120px;
    border-radius: 50%;
    overflow: hidden;
    cursor: pointer;
}

.avatar-image {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.upload-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,0.6);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    color: white;
    opacity: 0;
    transition: opacity 0.3s;
}

.avatar-container:hover .upload-overlay {
    opacity: 1;
}

.username {
    font-size: 24px;
    color: #303133;
    margin: 0;
}

.info-section {
    flex: 1;
    max-width: 600px;
}

.info-descriptions {
    margin-bottom: 30px;
}

.label-content {
    display: flex;
    align-items: center;
    gap: 8px;
    color: #606266;
}

.label-content .el-icon {
    font-size: 18px;
    color: #409EFF;
}

.info-text {
    color: #303133;
    font-size: 15px;
}

.edit-button {
    width: 100%;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    transition: all 0.3s;
}

.edit-button:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(64,158,255,0.3);
}

:deep(.el-descriptions__label) {
    background-color: #f5f7fa !important;
    padding: 16px !important;
}

:deep(.el-descriptions__content) {
    padding: 16px !important;
}

/* 响应式设计 */
@media (max-width: 768px) {
    .info-wrapper {
        flex-direction: column;
        align-items: center;
        padding: 20px;
    }

    .info-section {
        width: 100%;
    }
}

.password-input-group {
    display: flex;
    align-items: center;
    gap: 10px;
    width: 100%;
}

.password-input {
    width: 80%;
}

.edit-password-icon {
    font-size: 20px;
    color: #409EFF;
    cursor: pointer;
    transition: all 0.3s;
    flex-shrink: 0;
}

.edit-password-icon:hover {
    transform: scale(1.1);
    color: #66b1ff;
}

:deep(.el-form-item__content) {
    display: flex;
    align-items: center;
}

/* 确保所有输入框宽度一致 */
:deep(.el-input) {
    width: 80%;
}
</style>