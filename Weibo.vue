<template>
  <div>
       <p>登录中。。。</p>
        <p>{{code}}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
        code: this.$route.query.code
    };
  },
  methods: {},
  created() {
      this.axios.get('/userapp/weibo?code=' + this.code).then(res=>{
          alert(res.data.msg)
        //   sessionStorage.setItem('uid', res.data.uid)
          if (res.data.code == 202){
                alert('微博登录成功')
                this.$router.push({'path': '/index'})
            }else{
                alert('请继续绑定本地账号')
                this.$router.push({'path': '/weibobing', 'query': {'uid': res.data.uid}})
            }
      })
  },
};
</script>
</style>