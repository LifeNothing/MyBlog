<template>
  <div class="container">

    <div class="block">
      <div style="flex: 1"></div>
      <el-timeline class="card">
        <el-timeline-item :timestamp="blog.created" placement="top" v-for="blog in blogs">
          <el-card >
            <h4>
              <router-link :to="{name: 'BlogDetail', params: {blogId: blog.id}}">
                {{blog.title}}
              </router-link>
            </h4>
            <p>{{blog.description}}</p>
          </el-card>
        </el-timeline-item>
        <el-pagination class="mpage"
                       background
                       layout="prev, pager, next"
                       :current-page="currentPage"
                       :page-size="pageSize"
                       :total="total"
                       @current-change=page>
        </el-pagination>
      </el-timeline>
      <div style="flex: 1"></div>


    </div>

  </div>
</template>

<script>
import Header from '../components/Header'

export default {
  name: 'Blogs',
  components: {Header},
  data () {
    return {
      blogs: {},
      currentPage: 1,
      total: 0,
      pageSize: 5
    }
  },
  methods: {
    async page (currentPage) {
      const _this = this
      await _this.$axios.get('/blog/list?currentPage=' + currentPage).then(res => {
        _this.blogs = res.data.data.records
        _this.currentPage = res.data.data.current
        _this.total = res.data.data.total
        _this.pageSize = res.data.data.size
      })
      this.$store.state.newBlogs=_this.blogs
      console.log(this.$store.state.newBlogs)
    }
  },
  created () {
    this.page(1)
  }
}
</script>

<style scoped>
.container{

}
  .mpage {
    margin: 0 auto;
    text-align: center;
  }
.card{
  flex: 4;
}
.block{
    display: flex;
  }
</style>
