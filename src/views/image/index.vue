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
      <div class="action-head">
        <el-radio-group v-model="collect" size="mini" @change="loadImages(1)">
          <el-radio-button :label="false">全部</el-radio-button>
          <el-radio-button :label="true">收藏</el-radio-button>
        </el-radio-group>
        <el-button type="success" size="mini" @click="dialogUploadVisible=true">上传素材</el-button>
      </div>
      <!-- 素材列表 -->
      <el-row :gutter="10">
        <el-col :xs="12" :sm="6" :md="6" :lg="4" v-for="image in images" :key="image.id" class="image-item">
          <el-image style="height: 150px" :src="image.url" fit="cover"></el-image>
          <div class="image-action">
            <!--
            class 样式绑定
             {
                CSS类名: 布尔值
             }
             true：作用类名
             false：不作用类名
           -->
            <el-button type="warning" :icon="image.is_collected ? 'el-icon-star-on' : 'el-icon-star-off'" circle size="small" @click="onCollect(image)" :loading="image.loading"></el-button>
            <el-button size="small" type="danger" icon="el-icon-delete-solid" circle :loading="image.loading" @click="onDelete(image)"></el-button>

          </div>
        </el-col>
      </el-row>
      <!-- /素材列表 -->

      <!-- 数据分页 -->
      <!--
      分页数据改变以后，页码不会变化
      它需要单独处理高亮的页码-->
      <el-pagination background layout="prev, pager, next" :total="totalCount" :page-size="pageSize" :current-page.sync="page" @current-change="onPageChange">
      </el-pagination>
      <!-- /数据分页 -->

    </el-card>
    <el-dialog title="上传素材" :visible.sync="dialogUploadVisible" :append-to-body="true">
      <!--
        upload 组件本身就支持请求提交上传操作，说白了你使用它不用自己去发请求，它会自己发。
        请求方法：默认就是 POST
        请求路径：action，必须写完整路径
        请求头：headers -->

      <el-upload class="upload-demo" drag action="http://api-toutiao-web.itheima.net/mp/v1_0/user/images" multiple :headers="uploadHeaders" name="image" :on-success="onUploadSuccess" v-if="dialogUploadVisible">
        <i class="el-icon-upload"></i>
        <div class="el-upload__text">将文件拖到此处，或<em>点击上传</em></div>
        <div class="el-upload__tip" slot="tip">只能上传jpg/png文件，且不超过500kb</div>
      </el-upload>

    </el-dialog>

  </div>
</template>

<script>
import { getImages, collectImage, deleteImage } from '@/api/image'
export default {
  name: 'ImageIndex',
  components: {},
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
  created () {
    //页面初始化，加载第一页数据
    this.loadImages(1, false)
  },
  mounted () { },
  methods: {
    loadImages (page) {
      getImages({
        collect: this.collect,
        page,
        per_page: this.pageSize
      }).then(res => {
        const results = res.data.data.results
        results.forEach(img => {
          // img 对象本来没有 loading 数据
          // 我们这里收到的往里面添加该数据是用来控制每个收藏按钮的 loading 状态
          img.loading = false
        })
        this.images = results
        this.totalCount = res.data.data.total_count
        console.log(res)
      })
    },
    //根据全部收藏按钮刷新数据
    onCollectChange (value) {
      this.loadImages(1, value)
    },
    //上传关闭对话框，刷新数据列表
    onUploadSuccess () {
      this.dialogUploadVisible = false
      this.loadImages(this.page)
      this.$message({
        type: 'success',
        message: '上传成功'
      })
    },
    //分页响应
    onPageChange (page) {
      this.loadImages(page)
    },
    //图片收藏
    onCollect (image) {
      //展示loading
      image.loading = true
      collectImage(image.id, !image.is_collected).then(res => {
        //更新视图状态
        image.is_collected = !image.is_collected
        console.log(image)
        //关闭loading
        image.loading = false
      })
      this.loadImages
    },
    //图片删除
    onDelete (image) {
      image.loading = true
      deleteImage(image.id).then(res => {
        // 重新加载数据列表
        this.loadImages(this.page)
        image.loading = false
        this.$message({
          type: 'success',
          message: '删除图片成功'
        })
      })
    }
  }
}
</script>
<style scoped lang="less">
.action-head {
  padding-bottom: 20px;
  justify-content: space-between;
  display: flex;
}
.image-item {
  position: relative;
}

.image-action {
  font-size: 25px;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  color: #fff;
  height: 40px;
  background-color: rgba(204, 204, 204, 0.5);
  position: absolute;
  bottom: 4px;
  left: 5px;
  right: 5px;
}
</style>