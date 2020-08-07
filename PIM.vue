<template>
  <div>
      <Header></Header>
      <h1>个人信息页</h1>
      <a :href="'http://127.0.0.1:8000/userapp/invitecode?email='+ this.email" >生成邀请二维码</a>
      <hr>
      <div v-if="bian==0">
        <p v-for="user in userlist" :key="user.id">
          昵称：{{user.name}}<br>
          头像：<img :src="'http://127.0.0.1:8000/'+ user.img" width="60xp" height="60xp"><br>
          积分: {{user.integer}}<br>
          <input type="button" value="编辑" @click="bianji">

        </p>
      </div>
      <div v-else>
          昵称：<input type="text"  v-model="name"><br>
          头像：<input type="file" ref='img'><br>
          <input type="button" value="保存" @click="bao">
          <input type="button" value="取消" @click="quxiao">
      </div>
      
      <Footer></Footer>
  </div>
</template>

<script>
import Header from '@/components/Header'
import Footer from '@/components/Footer'
export default {
  data() {
    return {
        bian: 0,
        email: sessionStorage.getItem('email'),
        userlist:[],
        img:'',
        name:''
    };
  },
  components:{
        'Header':Header,
        'Footer':Footer,
  },
  methods: {
      bianji(){
          this.bian = 1

      },
      bao(){
          let fromdata = new FormData();
          fromdata.append('name', this.name)
          fromdata.append('img', this.$refs.img.files[0])
          fromdata.append('email', this.email)
          this.axios.post('/userapp/pim', fromdata).then(res=>{
              alert(res.data.msg)
              this.bian = 0
              this.$router.go(0)
          })
      },
      quxiao(){
          this.bian = 0
      }
  },
  created() {
      this.axios.get('/userapp/pim?email=' + this.email).then(res=>{
          this.userlist = res.data
      })
  },
};
</script>
