<template>
  <div id='detail2'>
      <!-- <h1>哈哈哈哈</h1> -->
      <table v-for="user in goodslist" :key="user.id">
        <tr>
          <td><h3>{{ user.teacher }}的店：</h3></td>
        </tr>
        <tr><td><h3><button @click="add_show">添加商品</button></h3></td></tr>
        <tr>
          <td>商品名称</td>
          <td>商品价格</td>
          <td>商品图片</td>
        </tr>
        <tr v-for="g in goodslist" :key="g.id">
          <td>{{g.title}}</td>
          <td>{{g.price}}</td>
          <td><img :src="'http://127.0.0.1:8000/'+g.picture" alt="" width="100xp" height="100xp"></td> 
          <td><button @click="del(g.id)">删除</button></td>
        </tr>
      </table>

      <div v-show="is_show" style="text-align:left">
          商品名称：<input type="text" v-model="name">
          商品价格：<input type="text" v-model="price">
          图片：<input type="file" id='img'>
          <button @click="sub">提交</button>
      </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
    name:'Detail2',
    data() {
        return {
            goodslist:[],
            is_show:false,
            name:'',
            price:''
        }
    },
    mounted() {
        // console.log(this.$route.query)
        let id=this.$route.query['cid']
        // console.log(id)
        
        axios({
            url:'http://127.0.0.1:8000/userapp/detail',
            method:'post',
            data:{'id':id}
        }).then((res)=>{
            this.goodslist=res.data.goodslist
            console.log(this.goodslist)
        })
    },
    methods: {
        del(id){
            let form = new FormData()
            form.append('id',id)
            axios({
                url:'http://127.0.0.1:8000/userapp/del',
                method:'post',
                data:form
            }).then((res)=>{
                alert(res.data.msg)
                window.location.reload() //刷新当前页面
            })
        },
        add_show(){
            this.is_show=true
        },
        sub(){
            let id=sessionStorage.getItem('tid')
            let form = new FormData()
            form.append('price',this.price)
            form.append('title',this.name)
            form.append('teacher',id)
            let img=document.getElementById('img').files[0]
            form.append('picture',img)
            axios({
                url:'http://127.0.0.1:8000/userapp/add',
                method:'post',
                headers:{'content-type':'multipart/form-data'},
                data:form
            }).then((res)=>{
                if(res.data.code==200){
                    alert('添加成功')
                    window.location.reload()
                }
            })
        }
    },
}
</script>

<style>

</style>