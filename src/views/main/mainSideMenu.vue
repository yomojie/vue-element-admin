<template>
  <aside :class="{'dark':sideMenuDarkSkin}" class="main-side-menu fl">
    <el-scrollbar>
      <el-menu
        ref="menu"
        @select="selectMenuHandle"
        :collapse="isCollapse"
        :collapse-transition="false"
        class="side-menu-root"
        :default-active="menuActiveIndex">
        <SubMenu ref="el_menu" v-for="menu in menuData" :key="menu.id" :menu="menu"></SubMenu>
      </el-menu>
    </el-scrollbar>
  </aside>
</template>

<script>
/**
   * Created by bigBigSir on 2019/3/28
   */
import SubMenu from './subMenu.vue'
import { mapState } from 'vuex'

export default {
  name: 'mainSideMenu',
  components: { SubMenu },
  data () {
    return {}
  },
  created () {
  },
  watch: {
    menuActiveIndex: function (n) {
      if (n === null) this.$refs.menu.activeIndex = n
    }
  },
  computed: {
    ...mapState(['menuData']),
    ...mapState('main', [
      'menuKey',
      'isCollapse',
      'menuActiveIndex',
      'sideMenuDarkSkin'
    ])
  },
  methods: {
    // 点击菜单项时触发
    selectMenuHandle (menuId) {
      const menu = this.findMenuItem(this.menuData, menuId)
      if (menu) this.$router.push(menu.routerPath)
    },
    // 依据菜单ID在菜单数据中查找该项菜单
    findMenuItem (menus, menuId) {
      let menu = null
      for (let i = menus.length; i--;) {
        if (menus[i].id === menuId) {
          menu = menus[i]
          break
        }
        if (menus[i].children && menus[i].children.length) {
          menu = this.findMenuItem(menus[i].children, menuId)
        }
      }
      return menu
    }
  }
}
</script>
