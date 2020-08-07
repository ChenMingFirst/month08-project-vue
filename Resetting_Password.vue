<template>
  <div>
      <h1>重置密码页面</h1>
      <p>新密码：<input type="password" v-model="passwd1"></p>
      <p>确认密码：<input type="password" v-model="passwd2"></p>
      <p><input type="button" value='重置密码' @click="resetting_password"></p>
  </div>
</template>

<script>
export default {
  data() {
    return {
        passwd1:'',
        passwd2:'',
        email: this.$route.query.email
    };
  },
  methods: {
      resetting_password(){
          const data ={
              'passwd1': this.passwd1,
              'passwd2': this.passwd2,
              'email': this.email
          }
          this.axios.post('/userapp/resetting_password', data).then(res=>{
              alert(res.data.msg)
              this.$router.push({'path': '/index'})
          })
      }
  },
  created() {},
};
</script>
</style>