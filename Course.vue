<template>
    <div>
        

        <div class="container layout layout-margin-top">
            <!-- 传递给子组件的方式：子组件要接受的变量='现有的变量' -->
            
            
            <div class="row">
                <div class="col-md-9 layout-body">
                    
            <div class="content">
                <div class="row course-cates">
                    <div class="col-md-1 course-cates-title">类别：</div>
                    <div class="col-md-11 course-cates-content" >
                        <a :class="{'active' : '' == choices.pay_choice}" @click="get_pay_id('')">全部</a>
                            <!-- <a v-if="pay.id == 3" target="_blank"><img src="/static/img/vip-icon.png"></a> -->
                            <!-- click.prevent 点击事件不再重新刷新页面 -->
                            <a  v-for="pay in pay_type" :key="pay.id" :class="{'active' : pay.id == choices.pay_choice}" @click="get_pay_id(pay.id)">{{pay.member}}</a>
                    </div>
                </div>
                <div class="row course-cates">
                    <div class="col-md-1 course-cates-title">标签：</div>
                    <div class="col-md-11 course-cates-content" style="text-align:left">
                        <a :class="{'active': '' == choices.lang_choice}"  @click="get_lang_id('')">全部</a>
                            <!-- 遍历分类标签 -->
                            <a :class="{'active': c_type.id == choices.lang_choice}" v-for="c_type in course_type" @click="get_lang_id(c_type.id)" :key="c_type.id">{{ c_type.name }}</a>
                    </div>
                </div>
            </div>
                <CourseBoby :choices="choices"></CourseBoby>
                </div>
                <div class="col-md-3 layout-side">
        <Header></Header>           


        <div class="panel panel-default panel-userinfo">
            <div class="panel-body body-userinfo">
                <div class="media userinfo-header">
                    <div class="media-left">
                        <div class="user-avatar">
                            
                            <a class="avatar" href="#sign-modal" data-toggle="modal" data-sign="signin">
                                <img class="circle" src="/static/img/logo-grey.png">
                            </a>
                            
                        </div>
                    </div>
                    <div class="media-body">
                        
                        <span class="media-heading username">欢迎来到实验楼</span>
                        <p class="vip-remain">做实验，学编程</p>
                        
                    </div>
                </div> 
                
                <div class="row userinfo-data">
                    
                    <hr>
                    <div class="btn-group-lr">
                    <a href="#sign-modal" type="button" class="btn btn-success col-md-5 col-xs-6 login-btn" data-toggle="modal" data-sign="signin">登录</a>
                    <a href="#sign-modal" type="button" class="btn btn-success col-md-5 col-xs-6 col-md-offset-1 register-btn" data-toggle="modal" data-sign="signup">注册</a>
                    </div>
                    
                </div>
                
                <div class="userinfo-footer row">
                    <div class="col-md-6 col-xs-6 pos-left">
                        
                        <a href="#sign-modal" data-toggle="modal" data-sign="signin"><span class="glyphicon glyphicon-bookmark"></span> 加入私有课</a>
                        
                    </div>
                    <div class="col-md-6 col-xs-6 pos-right">
                        <a href="/contribute" target="_blank"><span class="glyphicon glyphicon-send"></span> 我要投稿</a>
                    </div>

                    <div id="join-private-course" class="modal fade words-ctrl" tabindex="-1" role="dialog">
                        <div class="modal-dialog" role="document">
                            <div class="modal-content">
                                <div class="modal-header">
                                    <h4 class="modal-title">加入私有课</h4>
                                </div>
                                <div class="modal-body">
                                    <div style="margin:15px 0; font:16px;">输入教师提供的私有课程码可以加入教师的私有课程。</div>
                                    <form id="private-course-form" method="POST" action="/courses/join">
                                        <div class="form-group">
                                            <label for="code">邀请码</label>
                                            <input class="form-control" id="code" name="code" type="text" value="">
                                            <input name="_csrf_token" type=hidden value="1483780974##87f89328c5616669f00302a263fe9061bb852818">
                                        </div>
                                    </form>

                                </div>
                                <div class="modal-footer">
                                    <button type="button" class="btn btn-default" data-dismiss="modal">取消</button>
                                    <button type="button" class="btn btn-primary" onclick="document.getElementById('private-course-form').submit();">确定</button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>


        <div class="sidebox">
            
            <div class="sidebox-header">
                <h4 class="sidebox-title">最热路径</h4>
            </div>
            <div class="sidebox-body course-content side-list-body">
                <a href="/paths/python"><img style="width:25px;height:25px" src="/static/img/1471513769430.png"> Python 研发工程师</a>
                <a href="/paths/bigdata"><img style="width:25px;height:25px" src="/static/img/1471513926288.png"> 大数据工程师</a>
                <a href="/paths/cpp"><img style="width:25px;height:25px" src="/static/img/1471513793360.png"> C++ 研发工程师</a>
                <a href="/paths/security"><img style="width:25px;height:25px" src="/static/img/1471513867033.png"> 信息安全工程师</a>
                <a href="/paths/linuxsys"><img style="width:25px;height:25px" src="/static/img/1471514004752.png"> Linux 运维工程师</a>
            </div>
            
        </div>

        <div class="side-image side-qrcode">
            <img src="/static/img/ShiyanlouQRCode.png">
            <div class="side-image-text">关注公众号，手机看教程</div>
        </div>


                </div>
            </div>
        </div>


            <div class="modal fade" id="invite-user" tabindex="-1" role="dialog" aria-hidden="true">
                <div class="modal-dialog">
                    <div class="modal-content">
                        <div class="modal-header">
                            <button type="button" class="close" data-dismiss="modal"><span aria-hidden="true">&times;</span>
                            <span class="sr-only">Close</span></button>
                            <h4 class="modal-title">邀请好友，双方都可获赠实验豆！</h4>
                        </div>
                        <div class="modal-body">
                            
                                <h4><a href="#sign-modal" data-toggle="modal" data-sign="signin">登录</a>后邀请好友注册，您和好友将分别获赠3个实验豆！</h4>
                            
                            <div id="msg-modal"></div>
                        </div>
                    </div>
                </div>
            </div>


            <div class="modal fade" id="flash-message" tabindex="-1" role="dialog">
                <div class="modal-dialog" rolw="document">
                </div>
            </div>
            <div class="modal fade" id="modal-message" tabindex="-1" role="dialog" aria-hidden="true">
                <div class="modal-dialog">
                    <div class="modal-content">
                        <div class="modal-header">
                            <button type="button" class="close" data-dismiss="modal"><span aria-hidden="true">&times;</span><span class="sr-only">Close</span></button>
                            <h4 class="modal-title">注意</h4>
                        </div>
                        <div class="modal-body">
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-default" data-dismiss="modal">取消</button>
                            <button type="button" class="btn btn-primary confirm" data-dismiss="modal">确定</button>
                        </div>
                    </div>
                </div>
            </div>



            
                
                    
                
            

            <div class="modal fade" id="sign-modal" tabindex="-1" role="dialog">

            <div class="modal-dialog modal-sm" role="document">
                <!-- 登录注册 -->
                <LoginReg></LoginReg>
            </div>
        </div>

            <div id="base-data"
                
                
                    data-flash="false"
                    
                
                
                data-is-login=false
                
                data-is-jwt=true
                data-socket-url="wss://comet.shiyanlou.com"
                data-msg-user=""
                data-msg-system=""
            ></div>


        <div id="jinja-data"
            data-loginurl="/login"
            data-private-course-url="/courses/join"
            data-site-type="0"
            
        ></div>


        <Footer></Footer>
    </div>
</template>

<script>
import axios from 'axios'
import Header from '@/components/Header'
import Footer from '@/components/Footer'
import LoginReg from '@/components/LoginReg'
import CourseBoby from '@/components/CourseBoby'


export default {
    name:'Course',
    components:{
        'Header':Header,
        'Footer':Footer,
        'LoginReg':LoginReg,
        'CourseBoby':CourseBoby,
    },
    data() {
        return {
            course_type:[],
            pay_type:[],
            //定义向子组件传递的数据，为一个字典，为所需状态和语言选择
            choices:{
                pay_choice:'',
                lang_choice:''
            }

        }
    },
    //*** 在vue实例挂载时 的钩子函数
    mounted() {
        // 获取登录时存入的session
        let token = window.sessionStorage.getItem('token')
        let form_data = new FormData()
        form_data.append('token',token)
        axios({
            url:'http://127.0.0.1:8000/userapp/courses',
            method:'get',
            // 在请求头中修改Cookie
            headers:{
                'authorization':token,  //token获取不到 会传去一个字符串 [object Object] ***为什么
            },
        }).then((res)=>{
            // console.log(res.data.code)
            // 微博用户未绑定的情况
            if(res.data.code==20011){
                window.sessionStorage.clear()
            }
            if(res.data.code==20010 || res.data.code==20011){
                this.$router.push({
                    name:'Index',
                })
            }
            if(res.data.code==200){
                // console.log(res.data)
                this.course_type=res.data.course_type;
                this.pay_type = res.data.pay_type;
                // this.$router.go(0)
            }
        }).catch((res)=>{
            console.log(res)
        })
    },
    methods: {
        get_pay_id(id_){
            if(this.choices.pay_choice == id_){
                this.choices.pay_choice=''
                
                // sessionStorage.setItem('pay_choices',this.choices.pay_choice)
            }else{
                this.choices.pay_choice=id_
                
                // sessionStorage.setItem('pay_choices',this.choices.pay_choice)
            }
        },
        get_lang_id(id_){
            if(this.choices.lang_choice == id_){
                this.choices.lang_choice=''
                // console.log(this.choices)
            }else{
                this.choices.lang_choice=id_
                // console.log(this.choices)
            }
        }
    },

}
</script>

<style>

</style>
