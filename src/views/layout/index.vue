<template>
  <el-container class="layout-container">
    <el-aside class="aside" width="auto">
      <app-aside class="aside-menu"></app-aside>
    </el-aside>
    <el-container>
      <el-header class="header">
        <app-header></app-header>
      </el-header>
      <el-main class="main">
        <!-- 子路由出口 -->
        <router-view />
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
import AppAside from './components/aside'
import AppHeader from './components/header'
export default {
  name: 'LayoutIndex',
  components: { AppAside, AppHeader },
  props: {},
  data () {
    return {
    }
  },
  computed: {},
  watch: {},
  created () {

  },
  mounted () { },
  methods: {
  }
}
</script>

<style scoped lang="less">
.layout-container {
  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
}

.aside {
  background-color: #d3dce6;
  .aside-menu {
    height: 100%;
  }
}

.main {
  background-color: #e9eef3;
}
</style>