<template>
  <a-layout id="app-menu">
    <a-layout-sider
      theme="light"
      class="layout-sider"
    >
      <a-menu 
        theme="light" 
        mode="inline" 
        :selectedKeys="[current]"
        @menu-item-click="changeMenu">
        <a-menu-item v-for="(menuInfo, subIndex) in menu" :key="subIndex">
          <router-link :to="{ name: menuInfo.pageName, params: menuInfo.params}">
            <span>{{ menuInfo.title }}</span>
          </router-link>
        </a-menu-item>
      </a-menu>
    </a-layout-sider>
    <a-layout>
      <a-layout-content>
        <router-view />
      </a-layout-content>
    </a-layout>
  </a-layout>
</template>
<script>
// import { reactive } from 'vue'; 
import subMenu from '@/router/subMenu';

export default {
  props: {
    id: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      menu:{},
      //selectedKeys: ['menu_100'],
      current: 'menu_100',
      keys: []
    };
  },
  watch: {
    id: function () {
      console.log('watch id ----- ', this.id);
      this.current = 'menu_100';
      this.menuHandle();
    },
  },
  created () {
  },
  mounted () {
    this.menuHandle();
  },
  methods: {
    menuHandle () {
      // 该组件优先被加载了，所以没拿到参数
      console.log('params:', this.$route);
      console.log('menu ------ id:', this.id);
      this.menu = subMenu[this.id];
      const linkInfo = this.menu[this.current];
      this.$router.push({ name: linkInfo.pageName, params: linkInfo.params});
    },
    changeMenu(key) {
      this.current = key;
      this.menuHandle();
    }
  }
};
</script>
<style lang="less" scoped>
#app-menu {
  height: 100%;
  text-align: center;
  .layout-sider {
    border-top: 1px solid var(--color-neutral-3);
    border-right: 1px solid var(--color-neutral-3);
    // background-color: #FAFAFA;
    overflow: auto;
  }
}
</style>
