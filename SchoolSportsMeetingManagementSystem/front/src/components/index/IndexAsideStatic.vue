<template>
  <el-aside class="index-aside" width="200px">
    <div class="index-aside-inner menulist">
      <div 
        v-for="item in menuList" 
        :key="item.roleName" 
        v-if="role === item.roleName" 
        class="menulist-item"
      >
        <el-menu 
          :mode="menuMode" 
          :unique-opened="true" 
          class="el-menu-demo" 
          default-active="0"
          ref="menuRef"
        >
          <!-- 首页 -->
          <el-menu-item index="0" @click="menuHandler('')">
            <i v-if="false" class="el-icon-menu el-icon-s-home" />
            首页
          </el-menu-item>

          <!-- 个人中心（二级菜单） -->
          <el-submenu index="1">
            <template slot="title">
              <i v-if="false" class="el-icon-menu el-icon-user-solid" />
              个人中心
            </template>
            <el-menu-item index="1-1" @click="menuHandler('updatePassword')">修改密码</el-menu-item>
            <el-menu-item index="1-2" @click="menuHandler('center')">个人信息</el-menu-item>
          </el-submenu>

          <!-- 动态二级菜单 -->
          <el-submenu 
            v-for="(menu, index) in item.backMenu" 
            :key="menu.menu" 
            :index="index + 2 + ''"
          >
            <template slot="title">
              <i 
                v-if="false" 
                class="el-icon-menu" 
                :class="icons[index]" 
              />
              {{ menu.menu }}
            </template>
            <el-menu-item 
              v-for="(child, sort) in menu.child" 
              :key="sort" 
              :index="(index + 2) + '-' + sort" 
              @click="menuHandler(child.tableName)"
            >
              {{ child.menu }}
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </div>
    </div>
  </el-aside>
</template>

<script>
import menu from '@/utils/menu'
export default {
  data() {
    return {
      menuList: [],
      dynamicMenuRoutes: [],
      role: '',
      icons: [
        'el-icon-s-cooperation',
        'el-icon-s-order',
        'el-icon-s-platform',
        'el-icon-s-fold',
        'el-icon-s-unfold',
        'el-icon-s-operation',
        'el-icon-s-promotion',
        'el-icon-s-release',
        'el-icon-s-ticket',
        'el-icon-s-management',
        'el-icon-s-open',
        'el-icon-s-shop',
        'el-icon-s-marketing',
        'el-icon-s-flag',
        'el-icon-s-comment',
        'el-icon-s-finance',
        'el-icon-s-claim',
        'el-icon-s-custom',
        'el-icon-s-opportunity',
        'el-icon-s-data',
        'el-icon-s-check',
        'el-icon-s-grid',
        'el-icon-menu',
        'el-icon-chat-dot-square',
        'el-icon-message',
        'el-icon-postcard',
        'el-icon-position',
        'el-icon-microphone',
        'el-icon-close-notification',
        'el-icon-bangzhu',
        'el-icon-time',
        'el-icon-odometer',
        'el-icon-crop',
        'el-icon-aim',
        'el-icon-switch-button',
        'el-icon-full-screen',
        'el-icon-copy-document',
        'el-icon-mic',
        'el-icon-stopwatch',
      ],
      menuMode: 'vertical', // 强制垂直布局（根据需求调整）
      menulistBorderBottom: {},
    }
  },

  mounted() {
    // 加载菜单数据
    const menus = menu.list()
    if (menus) {
      this.menuList = menus
    } else {
      this.fetchMenuList()
    }
    this.role = this.$storage.get('role')
    this.initMenus()
  },

  created() {
    this.initMenus()
  },

  methods: {
    // 初始化菜单（核心：强制菜单高度 100%）
    initMenus() {
      this.icons.sort(() => 0.5 - Math.random())
      this.lineBorder()
      this.$nextTick(() => {
        this.forceMenuHeight() // 关键：强制菜单高度填满容器
        this.menulistStyleChange()
      })
    },

    // 强制菜单高度填满容器（终极解决底部空白）
    forceMenuHeight() {
      const menuEl = this.$refs.menuRef
      if (menuEl) {
        menuEl.style.height = '100%'
        menuEl.style.display = 'flex'
        menuEl.style.flexDirection = 'column'
      }
    },

    // 从接口获取菜单数据
    fetchMenuList() {
      const params = {
        page: 1,
        limit: 1,
        sort: 'id',
      }
      this.$http({
        url: 'menu/list',
        method: 'get',
        params,
      }).then(({ data }) => {
        if (data && data.code === 0) {
          this.menuList = JSON.parse(data.data.list[0].menujson)
          this.$storage.set('menus', this.menuList)
          this.initMenus() // 数据更新后重新初始化
        }
      })
    },

    // 处理菜单边框样式
    lineBorder() {
      const style = '${template.back.menulist.menulistStyle}'
      const w = '${template.back.menulist.menulistLineWidth}'
      const s = '${template.back.menulist.menulistLineStyle}'
      const c = '${template.back.menulist.menulistLineColor}'

      this.menulistBorderBottom = style === 'vertical' 
        ? { borderBottomWidth: w, borderBottomStyle: s, borderBottomColor: c } 
        : { borderRightWidth: w, borderRightStyle: s, borderRightColor: c }
    },

    // 菜单点击跳转
    menuHandler(name) {
      this.$router.push(`/${name}`)
    },

    // 样式动态调整
    menulistStyleChange() {
      this.setMenulistIconColor()
      this.setMenulistHoverColor()
      this.setMenulistStyleHeightChange()
    },

    // 图标颜色设置
    setMenulistIconColor() {
      this.$nextTick(() => {
        const arrows = document.querySelectorAll(
          '.menulist .el-submenu__title .el-submenu__icon-arrow'
        )
        arrows.forEach(el => {
          el.style.color = '${template.back.menulist.menulistIconColor}'
        })
      })
    },

    // 悬停颜色设置
    setMenulistHoverColor() {
      this.$nextTick(() => {
        const menuItems = document.querySelectorAll('.menulist .el-menu-item')
        const subTitles = document.querySelectorAll('.menulist .el-submenu__title')
        const hoverColor = '${template.back.menulist.menulistHoverColor}'

        menuItems.forEach(el => {
          el.addEventListener('mouseenter', e => {
            e.stopPropagation()
            el.style.backgroundColor = hoverColor
          })
          el.addEventListener('mouseleave', e => {
            e.stopPropagation()
            el.style.backgroundColor = 'transparent'
          })
        })

        subTitles.forEach(el => {
          el.addEventListener('mouseenter', e => {
            e.stopPropagation()
            el.style.backgroundColor = hoverColor
          })
          el.addEventListener('mouseleave', e => {
            e.stopPropagation()
            el.style.backgroundColor = 'transparent'
          })
        })
      })
    },

    // 高度设置（兼容旧逻辑）
    setMenulistStyleHeightChange() {
      this.$nextTick(() => {
        const menuItems = document.querySelectorAll(
          '.menulist-item>.el-menu--horizontal>.el-menu-item'
        )
        const subTitles = document.querySelectorAll(
          '.menulist-item>.el-menu--horizontal>.el-submenu>.el-submenu__title'
        )
        const height = '${template.back.menulist.menulistHeight}'

        menuItems.forEach(el => {
          el.style.height = height
          el.style.lineHeight = height
        })
        subTitles.forEach(el => {
          el.style.height = height
          el.style.lineHeight = height
        })
      })
    },
  },
}
</script>

<style lang="scss" scoped>
// 核心样式：100% 填满容器
.index-aside {
  position: relative;
  width: 200px;
  background-color: #0d102c;
  height: calc(100vh - 80px);
  overflow: hidden;

  .index-aside-inner {
    height: 100%;
    padding: 20px 0;
    overflow-y: auto;
    overflow-x: hidden;

    // 滚动条优化
    &::-webkit-scrollbar {
      width: 6px; 
      background: transparent;
    }
    &::-webkit-scrollbar-thumb {
      background: rgba(255, 255, 255, 0.2);
      border-radius: 3px;
    }
  }

  // 菜单样式穿透
  ::v-deep .el-menu-demo {
    border-right: none !important;
    height: 100% !important; // 强制菜单高度 100%

    .el-menu-item,
    .el-submenu__title {
      height: 48px;
      line-height: 48px;
      padding-left: 20px !important;
      color: #28ccd8 !important;
      background-color: #0d102c !important;
      transition: all 0.3s;

      &:hover {
        background: linear-gradient(to right, #0f75ff, #28cada) !important;
        color: #fff !important;
      }
    }

    .el-menu--inline {
      background-color: #0d102c !important;

      .el-menu-item {
        padding-left: 40px !important;
        color: #28ccd8 !important;

        &.is-active {
          background: linear-gradient(to right, #0f75ff, #28cada) !important;
          color: #fff !important;
          border-right: 3px solid #409EFF !important;
        }
      }
    }
  }

  // 消除底部空白
  ::v-deep .el-menu,
  ::v-deep .el-submenu {
    margin-bottom: 0 !important;
    padding-bottom: 0 !important;
  }
}

// 弹窗菜单样式
::v-deep .el-menu--horizontal .el-menu--popup {
  background-color: #0d102c !important;
  border: none !important;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1) !important;

  .el-menu-item {
    color: #28ccd8 !important;

    &:hover {
      background: linear-gradient(to right, #0f75ff, #28cada) !important;
      color: #fff !important;
    }
  }
}
</style>