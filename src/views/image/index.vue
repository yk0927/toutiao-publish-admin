<template>
  <div class="image-container">
    <el-card class="box-card">
      <div slot="header" class="clearfix">
        <!-- 面包屑路径导航 -->
        <el-breadcrumb separator-class="el-icon-arrow-right">
          <el-breadcrumb-item to="/">首页</el-breadcrumb-item>
          <el-breadcrumb-item>素材管理</el-breadcrumb-item>
        </el-breadcrumb>
        <!-- /面包屑路径导航 -->
        <!-- <el-button style="float: right; padding: 3px 0" type="text">操作按钮</el-button> -->
      </div>
      <ImageList />
    </el-card>
  </div>
</template>

<script>
import ImageList from './components/image-list.vue'
export default {
  name: 'ImageIndex',
  components: { ImageList },
  props: {},
  data () {
    const user = JSON.parse(window.localStorage.getItem('user'))
    return {
      images: [],//图片数据
      collect: false,//默认查询全部素材列表
      dialogUploadVisible: false,
      uploadHeaders: {
        Authorization: `Bearer ${user.token}`
      },//请求头
      totalCount: 0, // 总数据 条数
      pageSize: 18, // 每页大小
      page: 1, // 当前页码

    }
  },
  computed: {},
  watch: {},
  created () { },
  mounted () { },
  methods: {}
}
</script>
<style scoped lang="less">
</style>