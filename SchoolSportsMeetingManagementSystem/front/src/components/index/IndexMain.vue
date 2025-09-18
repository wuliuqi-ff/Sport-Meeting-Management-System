<template>
  <div class="page-container full-width"> 
    <el-main 
      class="main-content"
      :style="{
        'height': `calc(100% - ${template2?.back?.menulist?.menulistHeight || 60}px)`
      }"
    >
      <bread-crumbs :title="title" class="bread-crumbs"></bread-crumbs>
      <!-- 给router-view加一个唯一标识类，避免全局污染 -->
      <router-view class="router-view table-header-black" style="height:auto;overflow-y: auto;"></router-view>
    </el-main>
  </div>
</template>

<!-- 其他script部分完全不变 -->
<script>
import menu from "@/utils/menu";
export default {
  data() {
    return {
      menuList: [],
      role: "",
      currentIndex: -2,
      itemMenu: [],
      title: '',
    };
  },
  mounted() {
    let menus = menu.list();
    this.menuList = menus;
    this.role = this.$storage.get("role");
  },
  created() {
    this.init();
  },
  methods: {
    init() {
      this.$nextTick(() => {
        // 初始化逻辑
      })
    },
    menuHandler(menu) {
      this.$router.push({ name: menu.tableName });
      this.title = menu.menu;
    },
    titleChange(index, menus) {
      this.currentIndex = index;
      this.itemMenu = menus;
    },
    homeChange(index) {
      this.itemMenu = [];
      this.title = "";
      this.currentIndex = index;
      this.$router.push({ name: 'home' });
    },
    centerChange(index) {
      this.itemMenu = [
        { "buttons": ["新增", "查看", "修改", "删除"], "menu": "修改密码", "tableName": "updatePassword" },
        { "buttons": ["新增", "查看", "修改", "删除"], "menu": "个人信息", "tableName": "center" }
      ];
      this.title = "";
      this.currentIndex = index;
      this.$router.push({ name: 'home' });
    }
  }
};
</script>

<style lang="scss" scoped>
// 原有样式完全保留，只新增以下全局穿透样式
::v-deep .table-header-black {
  .el-table {
    .el-table__header {
      th {
        color: #000 !important; /* 强制表头文字为黑色 */
      }
    }
  }
}

// 以下为用户原有样式，完全不变
// 基础样式重置
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

// 页面容器
.page-container {
  width: 100%;
  min-width: 1312px; 
  margin: -90px -20px 0px; 
}

// 主内容区域
.main-content {
  padding: 0;
  background-image: url(/xiaoyunhuiguanli/img/back-list-img-bg.jpg);
  background-size: 10% 100%;
  background-repeat: repeat-x; 
  box-shadow: 0 0 6px rgba(0,0,0,0.05); 
  overflow: hidden;
  min-height: 100vh;
}

// 导航链接样式
a {
  text-decoration: none;
  color: #555;
  transition: color 0.2s ease;

  &:hover {
    color: var(--publicMainColor, #4b6973); 
    background: transparent; 
  }
}

// 导航列表样式
.nav-list {
  width: 100%;
  margin: 0 auto;
  text-align: left;
  margin-top: 20px;

  .nav-title {
    display: inline-block;
    font-size: 15px;
    color: #333;
    padding: 12px 25px; 
    border: none;
    border-radius: 2px; 
    cursor: pointer;
    transition: all 0.2s ease;

    &:hover {
      background-color: rgba(75, 105, 115, 0.05); 
    }

    &.active {
      color: var(--publicMainColor, #4b6973);
      background-color: #fff;
      box-shadow: 0 2px 4px rgba(0,0,0,0.05); 
      font-weight: 500; 
    }
  }
}

// 操作项样式
.nav-item {
  margin-top: 15px; 
  background: #FFFFFF;
  padding: 15px 0;
  border-radius: 4px; 
  box-shadow: 0 1px 3px rgba(0,0,0,0.03); 

  .menu {
    padding: 12px 25px; 
    transition: background-color 0.2s;

    &:hover {
      background-color: rgba(75, 105, 115, 0.03);
    }
  }
}

// 路由视图容器
.router-view {
  padding: 20px; 
  margin: 15px; 
  background: #FFFFFF;
  border-radius: 6px; 
  box-shadow: 0 2px 8px rgba(0,0,0,0.05); 
}

// 面包屑导航
.bread-crumbs {
  width: 100%;
  padding: 15px 20px; 
  color: #666;
  font-size: 14px;
  border-bottom: 1px solid #f0f0f0; 

  ::v-deep .el-breadcrumb__separator {
    margin: 0 8px;
    color: #ccc;
  }
}

// 详情表单样式
.detail-form-content {
  background: transparent;
}

// 全局滚动与溢出控制
html, body {
  overflow-x: hidden; 
  background-color: #f9f9f9; 
}

// 按钮样式优化（纯蓝色主题 + 增强交互）
::v-deep .router-view {
  .el-button {
    margin: 10 5px !important; 
    height: 28px !important; 
    padding: 0 3px !important; 
    border-radius: 2px !important; 
    transition: all 0.2s ease !important;
    border: 6px solid black transparent !important;
    font-size: 14px !important;

    &:hover {
      transform: translateY(-1px); 
      box-shadow: 0 2px 6px rgba(30, 144, 255, 0.2) !important; 
    }

    &:active {
      transform: translateY(0); 
    }
  }

  .el-button--success,
  .el-button--primary,
  .el-button--danger {
    color: #fff !important;
    background-color: #0074e7 !important; 
    border-color: #0074e7 !important;

    &:hover {
      background-color: #1888e6 !important; 
      border-color: #1888e6 !important;
    }
  }
}

// 表格样式（保持原有，仅通过上方新增的::v-deep .table-header-black修改表头颜色）
::v-deep .el-table {
  border-radius: 4px;
  overflow: hidden; 
  border: 1px solid #f0f0f0;

  ::v-deep .el-table__header {
    background-color: #f7f9fc;

    th {
      background-color: #f7f9fc !important;
      color: #555 !important; /* 原有颜色，被上方新增样式覆盖 */
      font-weight: 500;
      border-bottom: 1px solid #e8e8e8 !important;
    }
  }

  ::v-deep .el-table__body {
    tr {
      &:hover > td {
        background-color: #f5faff !important; 
      }

      &.el-table__row--striped {
        background-color: #fafafa !important;
      }
    }
  }

  ::v-deep .el-table__cell {
    border-right: 1px solid #f0f0f0 !important;

    &:last-child {
      border-right: none !important;
    }
  }

  ::v-deep .el-table__row {
    border-bottom: 1px solid #f0f0f0 !important;
  }
}

// 分页样式优化
::v-deep .el-pagination {
  margin-top: 20px;
  padding: 10px 0;
  text-align: right;

  ::v-deep .el-pagination__sizes .el-input .el-input__inner {
    height: 30px;
    border-radius: 2px;
  }

  ::v-deep .el-pager li {
    margin: 0 2px;
    border-radius: 2px;

    &.active {
      background-color: #1E90FF;
      color: #fff;
    }
  }
}
</style>