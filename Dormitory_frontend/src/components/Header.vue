<template>
  <div class="header-container">
    <div class="logo">
      小白的宿舍管理系统
    </div>
    <div class="clock-container">
      <Clock class="clock" />
    </div>
    <div class="spacer"></div>
    <div class="right-info">
      <el-dropdown>
        <span class="el-dropdown-link">
          <el-icon :size="20" class="avatar-icon"><avatar /></el-icon>
          <span class="username">个人中心</span>
          <el-icon class="arrow-icon"><arrow-down /></el-icon>
        </span>
        <template #dropdown>
          <el-dropdown-menu>
            <el-dropdown-item @click="selfInfoManage" class="dropdown-item">
              <el-icon><user /></el-icon>
              个人信息
            </el-dropdown-item>
            <el-dropdown-item @click="SignOut" class="dropdown-item">
              <el-icon><switch-button /></el-icon>
              退出登录
            </el-dropdown-item>
          </el-dropdown-menu>
        </template>
      </el-dropdown>
    </div>
  </div>
</template>

<script>

import request from "@/utils/request";
import Clock from "@/components/Clock";

const {ElMessage} = require("element-plus");

export default {
  name: "Header",
  components: {
    Clock
  },
  data() {
    return {
      name: '',
    }
  },
  created() {
  },
  methods: {
    SignOut() {
      sessionStorage.clear()
      request.get("/main/signOut");
      ElMessage({
        message: '用户退出登录',
        type: 'success',
      });
      this.$router.replace({path: '/login'});
    },
    selfInfoManage() {
      this.$router.push("/selfInfo")
    }
  },
}
</script>

<style scoped>
.header-container {
  display: flex;
  align-items: center;
  height: 50px;
  padding: 0 14px;
  background: #ffffff;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
}

.logo {
  font-size: 24px;
  font-weight: bold;
  color: #409eff;
  margin-right: 40px;
}

.clock-container {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}

.clock {
  font-size: 18px;
  color: #666;
}

.spacer {
  flex: 1;
}

.right-info {
  position: relative;
}

.el-dropdown-link {
  display: flex;
  align-items: center;
  padding: 8px 12px;
  border-radius: 4px;
  transition: all 0.3s;
}

.el-dropdown-link:hover {
  background: #f5f7fa;
  cursor: pointer;
}

.avatar-icon {
  margin-right: 8px;
  color: #409eff;
}

.username {
  font-size: 14px;
  color: #606266;
}

.arrow-icon {
  margin-left: 4px;
  color: #909399;
}

.dropdown-item {
  display: flex;
  align-items: center;
  padding: 8px 16px;
}

.dropdown-item .el-icon {
  margin-right: 8px;
}
</style>