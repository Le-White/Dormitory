<template>
    <div class="page-container">
       
        <div class="login-container">
            <div class="login-card">
                <div class="card-content">
                    <div class="title">欢迎使用小白的宿舍管理系统</div>
                    <el-form ref="form" :model="form" :rules="rules" size="large">
                        <el-form-item prop="username">
                            <el-input 
                                v-model="form.username" 
                                clearable 
                                prefix-icon="avatar"
                                placeholder="请输入账号"
                            ></el-input>
                        </el-form-item>
                        <el-form-item prop="password">
                            <el-input 
                                v-model="form.password" 
                                prefix-icon="lock" 
                                show-password
                                placeholder="请输入密码"
                            ></el-input>
                        </el-form-item>
                        <el-form-item :model="form" prop="identity" class="role-select">
                            <el-radio-group v-model="form.identity" class="role-group">
                                <el-radio :value="'stu'">
                                    <span class="role-label">学生</span>
                                </el-radio>
                                <el-radio :value="'dormManager'">
                                    <span class="role-label">宿舍管理员</span>
                                </el-radio>
                                <el-radio :value="'admin'">
                                    <span class="role-label">超级管理中心</span>
                                </el-radio>
                            </el-radio-group>
                        </el-form-item>
                        <el-form-item>
                            <el-button 
                                :disabled="!disabled" 
                                type="primary" 
                                class="login-button"
                                @click="login"
                            >
                                登 录
                            </el-button>
                        </el-form-item>
                    </el-form>
                </div>
            </div>
            <div class="footer">
                @小白不会敲代码
            </div>
        </div>
    </div>
</template>
<script src="@/assets/js/Login.js"></script>
<style scoped>
@import "../assets/css/Login.css";

</style>