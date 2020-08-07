<template>
  <div id="coursebody">
      {{ courses }}
                <div class="content position-relative">
                <ul class="nav nav-tabs" role="tablist">
                    <li class="active"><a href="/courses?course_type=all&amp;tag=all&amp;fee=all">已上线</a></li>
                    <li class=""><a href="/courses?status=preview" class="stat-event" data-stat="preview_course">即将上线</a></li>
                </ul>
                <div class="clearfix"></div>
                <div class="courses-sort">
                    <a  href="/courses/?course_type=all&amp;tag=all&amp;fee=all&amp;order=latest">最新</a>
                    
                    <a  href="/courses/?course_type=all&amp;tag=all&amp;fee=all&amp;order=hotest">最热</a>
                </div>
                <div class="search-result"></div>
                <div class="row">
                
                    <!-- 遍历课程 -->
                    <div v-for="c in course" class="col-md-4 col-sm-6  course" :key="c.id">
                        <a class="course-box" @click="detail(c.id,c.teacher)">
                            <div class="sign-box">
                                
                                <i class="fa fa-star-o course-follow pull-right"
                                    data-follow-url="/courses/63/follow"
                                    data-unfollow-url="/courses/63/unfollow"  style="display:none" >
                                </i>
                                
                            </div>
                            <div class="course-img">
                                <img :alt="c.title" :src="'http://127.0.0.1:8000/'+c.picture">
                            </div>
                            
                            <div class="course-body">
                                <span class="course-title" data-toggle="tooltip" data-placement="bottom" :title="c.description">{{c.title}}</span>
                            </div>
                            <div class="course-footer">
                                <span class="course-per-num pull-left">
                                    <i class="fa fa-users"></i>
                                    {{c.count}}
                                </span>
                                <button class="course-bootcamp pull-right" @click.stop.prevent="follow(c.id)">{{ follow_data }}</button>
                                <span class="course-bootcamp pull-right">教师：{{c.teacher}}</span>
                            </div>
                        </a>
                    </div>

                </div>
                
                <!-- 分页 -->
                <nav class="pagination-container">
                    <ul class="pagination">
                        
                        <li class="disabled">
                            <a href="#" aria-label="Previous">
                                <span aria-hidden="true">上一页</span>
                            </a>
                        </li>
                        
                    
                        <li class="active">
                            <a href="">1</a>
                        </li>
                        
                        <li class="">
                            <a href="">2</a>
                        </li>
                                  
                        <li class="">
                            <a aria-label="Next" href="">
                                <span aria-hidden="true">下一页</span>
                            </a>
                        </li>
                        
                    </ul>
                </nav>

            </div>

  </div>
</template>

<script>
import axios from 'axios'
export default {
    name:'CourseBoby',
    data() {
        return {
            course:[],
            follow_data:'关注',
            page_num:1, //默认为第一页
        }
    },
    computed: {
        courses(){
            let token=sessionStorage.getItem('token')
            axios({
            url:'http://127.0.0.1:8000/userapp/getcourse',
            method:'get',
            headers:{'authorization':token}
            }).then((res)=>{
                this.course=res.data.course
                // console.log(this.$router.params)
            })
        }
    },
    watch: {
        choices:{ //只有通过这样的方法才能深处监听
            handler(oldval,newval){
                let form = new FormData()
                let token=sessionStorage.getItem('token')
                form.append('pay_choice',this.choices.pay_choice)
                form.append('lang_choice',this.choices.lang_choice)
                
                axios({
                    url:'http://127.0.0.1:8000/userapp/getcourse',
                    method:'post',
                    data:form,
                    headers:{'authorization':token}
                }).then((res)=>{
                    if(res.data.code==200){
                        // console.log(res.data)
                        this.course=res.data.course_list
                        // this.$router.go(0)
                    }
                })
            },
            deep:true
        }
    },
    //props:用于接收父组件传递给子组件的值
    props: {
        choices:{
            type:Object,//是一个对象，类似python的字典
            required:true
        }
    },
    methods: {
        follow(cid){
            let token=sessionStorage.getItem('token')
            let form = new FormData()
            form.append('cid',cid)
            axios({
                method:'post',
                url:'http://127.0.0.1:8000/userapp/follow',
                data:form,
                headers:{'authorization':token}
            }).then(res=>{
                alert(res.data.msg)
            })
        },
        detail(cid,tid){//使用query方式传值
            // alert(id_)
            sessionStorage.setItem('tid',tid)
            this.$router.push({path:'/Detail',query:{'tid':tid,'cid':cid}})
        }
    },
    mounted() {
        let form = new FormData()
        form.append('page_num',this.page_num)
        axios({
            url:'http://127.0.0.1:8000/api/user/pag/',
            method:'post',
            data:form
        })
    },

}
</script>

<style>

</style>