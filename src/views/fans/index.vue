<template>
  <div class="fans-container">
    <el-card>
      <!-- 头部 -->
      <div slot="header">
        <el-breadcrumb separator-class="el-icon-arrow-right">
          <el-breadcrumb-item to="/">首页</el-breadcrumb-item>
          <el-breadcrumb-item>粉丝管理</el-breadcrumb-item>
        </el-breadcrumb>
      </div>
      <!-- 内容：选项卡 -->
      <el-tabs type="border-card">
        <!-- 粉丝列表 -->
        <el-tab-pane label="粉丝列表">
          <el-row>
            <el-col :span="3">
              <el-card class="fans-card" v-for="fans in fansList" :key="fans.id">
                <img class="fans-photo" :src="fans.photo" />
                <span>{{ fans.name }}</span>
                <el-button type="primary">+关注</el-button>
              </el-card>
            </el-col>
          </el-row>
          <!-- 分页导航 -->
          <el-pagination background :current-page.sync="currentPage" :page-size="pageSize" layout="total, prev, pager, next" :total="totalCount" @current-change="loadFansList" />
        </el-tab-pane>
        <!-- 粉丝画像 -->
        <el-tab-pane label="粉丝画像">
          <FansCharts />
        </el-tab-pane>
        <el-tab-pane label="地图Demo">
          <MapDemo />
        </el-tab-pane>
         <el-tab-pane label="地图Bmap">
          <Bmap />
        </el-tab-pane>
      </el-tabs>
    </el-card>
  </div>
</template>

<script>
import { getFansList } from '@/api/fans'
import FansCharts from './components/FansCharts.vue'
import MapDemo from './components/map-demo.vue'
import Bmap from './components/Bmap.vue'
export default {
  name: 'FansIndex',
  components: {
    FansCharts,MapDemo,Bmap
  },
  props: {},
  data () {
    return {
      // activeName: 'first', // 当前激活卡片页名称
      fansList: [], // 粉丝列表
      totalCount: 0,
      currentPage: 1,
      pageSize: 21
    }
  },
  computed: {},
  watch: {},
  created () {
    // 初始化粉丝列表
    this.loadFansList()
  },
  mounted () { },
  methods: {
    async loadFansList (page = 1) {
      this.currentPage = page
      const {
        data: { data: fansListInfo }
      } = await getFansList({
        page,
        per_page: this.pageSize
      })
      this.fansList = fansListInfo.results
      this.totalCount = fansListInfo.total_count
    }
  }
}
</script>

<style lang="less" scoped>
.fans-container {
  .fans-card {
    /deep/.el-card__body {
      height: 150px;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      align-items: center;
    }
  }
}
</style>
