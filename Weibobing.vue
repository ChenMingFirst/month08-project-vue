<template>
  <div>
      <p>请继续绑定本地账号</p>
      <p>{{uid}}</p>
      邮箱：<input type="text" v-model='email'>
      密码：<input type="password" v-model="password">
      <input type="button" value="绑定" @click="bing">
  </div>
</template>

<script>
export default {
    data() {
        return{
            uid: this.$route.query.uid,
            email: '',
            password: ''
        }
    },
    methods: {
        bing(){
            const data = {
                'email': this.email,
                'password': this.password,
                'uid': this.uid
            }
            this.axios.post('/userapp/weibo', data).then(res=>{
                alert(res.data.msg)
                this.$router.push({'path': '/index'})
            }).catch(error=>{
                alert(error.response)
            })
        }
    }
}
</script>