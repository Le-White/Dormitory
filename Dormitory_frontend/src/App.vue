<template>
  <div class="app"  title="宿舍管理系统">
    <router-view/>
  </div>
</template>

<style>
/* 重置基础样式 */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: #f5f7fa;
  /* 禁用页面复制功能 */
  user-select: none; /* 标准语法 */
    -webkit-user-select: none; /* 兼容 Chrome/Safari */
    -moz-user-select: none; /* 兼容 Firefox */
    -ms-user-select: none; /* 兼容 IE */
}

.app {
  min-height: 100vh;
}


</style>

<script>
export default {
  name: "Layout",
}
</script>
