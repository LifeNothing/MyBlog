<template>
  <div class="m-content">
   <div style="flex: 1">
      <div class="user" @click="login">管理员登录</div>
    </div>
    <div class="header">
      <img src="../assets/logo.png" alt="">
      <el-tabs class="tabs" v-model="activeName" @tab-click="handleClick">
        <el-tab-pane label="首页" name="first"></el-tab-pane>
        <el-tab-pane label="留言本" name="second"></el-tab-pane>
        <el-tab-pane label="搜索" name="third"></el-tab-pane>
      </el-tabs>
    </div>
    <div style="flex: 1"></div>
  </div>

</template>

<script>
  // import '../assets/css/element-variables.scss'
  export default {
    name: "Header",
    data() {
      return {
        activeName: 'first',
        user: {
          username: '请先登录',
          avatar: 'https://cube.elemecdn.com/3/7c/3ea6beec64369c2642b92c6726f1epng.png'
        },
        hasLogin: false
      }
    },
    methods: {
      login(){
        this.$router.push('/login')
      },
      handleClick(tab){
        switch (tab.name) {
          case 'first' : this.$router.push('/blog'); break
          case 'second' : this.$router.push('/blog/add'); break
        }
      },
      logout() {
        const _this = this
        _this.$axios.get("/logout", {
          headers: {
            "Authorization": localStorage.getItem("token")
          }
        }).then(res => {
          _this.$store.commit("REMOVE_INFO")
          _this.$router.push("/login")

        })
      }
    },
    created() {
      if(this.$store.getters.getUser.username) {
        this.user.username = this.$store.getters.getUser.username
        this.user.avatar = this.$store.getters.getUser.avatar
        this.hasLogin = true
      }
    }
  }
</script>

<style scoped>
  .m-content {
    width: 100%;
    height: 71px;
    position: fixed;
    display: flex;
    top: 0;
    z-index: 10000;
    border-bottom: 1px solid #ccc;
    background-color: white;
    overflow:hidden;
  }
  .header{
    width: 1080px;
    flex: 3;
  }
  .m-content img{
    height: 50px;
    float: left;
    line-height: 70px;
    margin-top: 10px;
    margin-left: 5%;
  }
  .tabs{
    float: right;
    line-height: 70px;
    margin-right: 5%;
  }
  .user{
    width: 80px;
    line-height: 70px;
    color: #ccc;
  }
</style>
