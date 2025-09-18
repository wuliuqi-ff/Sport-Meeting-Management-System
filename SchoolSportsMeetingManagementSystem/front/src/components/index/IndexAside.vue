<template>
  <el-aside class="index-aside" width="200px">
    <div class="index-aside-inner">
      <el-menu default-active="1">
        <el-menu-item @click="menuHandler('/')" index="1">
          <!-- <i class="el-icon-s-home"></i> -->
          首页
        </el-menu-item>
        <sub-menu
          v-for="menu in menuList"
          :key="menu.menuId"
          :menu="menu"
          :dynamicMenuRoutes="dynamicMenuRoutes"
        ></sub-menu>
      </el-menu>
    </div>
  </el-aside>
</template>
<script>
import SubMenu from "@/components/index/IndexAsideSub";
export default {
  data() {
    return {
      menuList: [],
      dynamicMenuRoutes: []
    };
  },
  components: {
    SubMenu
  },
  mounted() {
    // 获取动态菜单数据并且渲染
    this.menuList = JSON.parse(sessionStorage.getItem("menuList") || "[]");
    this.dynamicMenuRoutes = JSON.parse(
      sessionStorage.getItem("dynamicMenuRoutes") || "[]"
    );
    
    // 确保菜单渲染后调整高度
    this.$nextTick(() => {
      this.adjustSidebarHeight();
    });
  },
  methods: {
    menuHandler(path) {
      this.$router.push({ path: path });
    },
    adjustSidebarHeight() {
      const sidebar = document.querySelector('.index-aside');
      if (sidebar) {
        // 设置侧边栏高度为视口高度减去顶部导航高度
        sidebar.style.height = `calc(100vh - 80px)`;
      }
    }
  }
};
</script>
<style lang="scss" scoped>
.index-aside {
  // 使用绝对定位和计算高度替代 margin-top
  position: fixed;
  top: 80px;
  bottom: 0;
  width: 200px;
  overflow: hidden;
  
  .index-aside-inner {
    // 修复宽度，避免水平滚动条
    width: 100%;
    height: 100%;
    overflow-y: auto;
    
    // 美化滚动条
    &::-webkit-scrollbar {
      width: 6px;
    }
    &::-webkit-scrollbar-track {
      background: transparent;
    }
    &::-webkit-scrollbar-thumb {
      background: rgba(0, 0, 0, 0.2);
      border-radius: 3px;
    }
  }
}
</style>