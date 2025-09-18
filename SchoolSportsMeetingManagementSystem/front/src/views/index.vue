<template>
  <el-container class="app-container">
    <!-- 顶部导航 -->
    <el-header class="index-header">
      <index-header />
    </el-header>
    
    <!-- 主体内容区 -->
    <el-container class="main-content-container">
      <!-- 侧边栏 -->
      <el-aside class="index-aside" width="200px">
        <index-aside />
      </el-aside>
      
      <!-- 主内容 -->
      <el-main class="index-main">
        <index-main />
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
import IndexHeader from '@/components/index/IndexHeader'
import IndexAside from '@/components/index/IndexAsideStatic'
import IndexMain from '@/components/index/IndexMain'

export default {
  name: 'Layout',
  components: {
    IndexHeader,
    IndexAside,
    IndexMain
  }
}
</script>

<style lang="scss" scoped>
// 重置基础样式
*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

// 确保页面占满整个视口
html, body {
  height: 100%;
  width: 100%;
  margin: 0;
  padding: 0;
  overflow: hidden; // 防止全局滚动
}

// 应用容器
.app-container {
  display: flex;
  flex-direction: column;
  height: 100vh; // 占满整个视口高度
  width: 100%;
}

// 顶部导航样式
.index-header {
  height: 80px; // 明确顶部导航高度
  background-color: #f5f7fa; // 根据实际需求调整
  border-bottom: 1px solid #ebeef5; // 添加底部边框
  flex-shrink: 0; // 防止被压缩
}

// 主要内容区域容器
.main-content-container {
  display: flex;
  flex: 1; // 填充剩余空间
  position: relative;
}

// 侧边栏样式
.index-aside {
  height: 100%; // 填充父容器高度
  background-color: #0d102c; // 根据实际需求调整
  overflow-y: auto; // 内容超出时显示滚动条
  
  // 美化滚动条
  &::-webkit-scrollbar {
    width: 6px;
  }
  &::-webkit-scrollbar-track {
    background: transparent;
  }
  &::-webkit-scrollbar-thumb {
    background: rgba(255, 255, 255, 0.2);
    border-radius: 3px;
  }
}

// 主内容区域样式
.index-main {
  flex: 1; // 填充剩余空间
  overflow-y: auto; // 内容超出时显示滚动条
  padding: 20px; // 添加内边距，提升可读性
  background-color: #ffffff; // 根据实际需求调整
}
</style>