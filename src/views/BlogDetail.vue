<template>
  <div class="container">
  <div class="detail-contain">

    <div class="mblog">
      <h2> {{ blog.title }}</h2>
      <el-link icon="el-icon-edit" v-if="ownBlog">
        <router-link :to="{name: 'BlogEdit', params: {blogId: blog.id}}" >编辑</router-link>
      </el-link>
      <el-divider></el-divider>
      <div class="markdown-body" v-html="blog.content"></div>
    </div>

    <div class="detail-side">
      <div class="side-label">
        <div class="label-list">最新文章</div>
        <div class="label-button">
          <div class="button" v-for="blog in this.$store.state.newBlogs">
              <el-button @click="toBlogDetail(blog.id)">{{blog.title}}</el-button>
          </div>
        </div>
      </div>
      <div class="leaveMessage-side">
        <div class="label-list">最新留言</div>
      </div>
    </div>

  </div>
  </div>
</template>

<script>
  import 'github-markdown-css'
  import Header from "../components/Header";
  export default {
    name: "BlogDetail.vue",
    components: {Header},
    data() {
      return {
        blog: {
          id: "",
          title: "",
          content: ""
        },
        ownBlog: false,
      }
    },
    methods: {
      toBlogDetail(id){
        this.$router.push('/blog/'+id)
        console.log(id)
      }
    },
    created() {
      const blogId = this.$route.params.blogId
      console.log(blogId)
      const _this = this
      this.$axios.get('/blog/' + blogId).then(res => {
        const blog = res.data.data
        _this.blog.id = blog.id
        _this.blog.title = blog.title

        var MardownIt = require("markdown-it")
        var md = new MardownIt()

        var result = md.render(blog.content)
        _this.blog.content = result
        _this.ownBlog = (blog.userId === _this.$store.getters.getUser.id)

      })
    }
  }
</script>

<style scoped>
  .detail-contain{
    width: 1300px;
    margin: 0 auto;
    display: flex;
  }

  .mblog {
    flex: 3;
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
    width: 733px;
    min-height: 700px;
    padding: 20px 15px;
  }
  .detail-side{
    flex: 1;
    width: 300px;
    height: 700px;
    margin-left: 30px;
  }
  .side-label{
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
    padding: 20px;
    width: 250px;
    height: 320px;

  }
  .label-list{
    font-size: 21px;
    padding: 0 0 10px;
    margin-bottom: 10px;
    border-bottom: 1px solid #e4e8eb;
  }
  .label-button{
    overflow: hidden;
  }
.leaveMessage-side{
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
  padding: 20px;
  width: 250px;
  height: 320px;
  margin-top: 10px;
}
  .button{
    display: inline-block;
    margin-top: 10px;
    margin-left: 10px;
  }
</style>
