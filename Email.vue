<template>
  <div>
      <p>请输入邮箱号：</p>
      <input type="text" v-model="email">
      <input type="button" value="点击发送邮件，重置密码" @click="send_email">
  </div>
</template>

<script>
export default {
  data() {
    return {
        email:''
    };
  },
  methods: {
      send_email(){
          this.axios.post('/userapp/send_email', {'email': this.email}).then(res=>{
              alert(res.data.msg)
          })
      }
  },
  created() {},
};
</script>
</style>