<template>
    <div>
        <div class="modal-content">
            <button type="button" class="close-modal" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
            <div class="modal-body">
                <ul class="nav nav-tabs" role="tablist">
                    <li role="presentation" class="active">
                        <a href="#signin-form" aria-controls="signin-form" role="tab" data-toggle="tab">登录</a>
                    </li>
                    <li role="presentation">
                        <a href="#signup-form" aria-controls="signup-form" role="tab" data-toggle="tab">注册</a>
                    </li>
                </ul>
                        <div class="tab-content">
                    <div role="tabpanel" class="tab-pane active" id="signin-form">
                        <form>
                            <div class="form-group">
                                <div class="input-group">
                                    <div class="input-group-addon">
                                        <i class="fa fa-envelope" style="margin:0;"></i>
                                    </div>
                                    <input type="email" v-model="login_email" class="form-control" placeholder="请输入邮箱">
                                </div>
                            </div>
                            <div class="form-group">
                                <div class="input-group">
                                    <div class="input-group-addon">
                                        <i class="fa fa-lock" style="margin:0;"></i>
                                    </div>
                                    <input type="password" v-model="login_passwd" class="form-control" placeholder="请输入密码">
                                </div>
                            </div>
                            <div class="form-inline verify-code-item" style="display:none;">
                                <div class="form-group">
                                    <div class="input-group">
                                        <input type="text" class="form-control" placeholder="请输入验证码">
                                    </div>
                                </div>
                                <!-- <img class="verify-code" src="" source="https://www.shiyanlou.com/captcha.gif"> -->
                                
                            </div>

                                <div class="help-block text-left">{{login_message}}</div>

                            <div class="form-group remember-login">
                                <input name="remember" type="checkbox" value="y"> 下次自动登录
                                <a class="pull-right" href="/email">忘记密码？</a>
                            </div>
                            <div class="form-group">
                                <input class="btn btn-primary" @click="login" type="button" value="进入实验楼">
                            </div>
                            <div class="form-group widget-signin">
                                <span>快速登录</span>
                                <a href="/auth/qq?next="><i class="fa fa-qq"></i></a>
                                <a href="https://api.weibo.com/oauth2/authorize?client_id=2028331105&redirect_uri=http://127.0.0.1:8080/weibo"><i class="fa fa-weibo"></i></a>
                                <a href="/auth/weixin?next="><i class="fa fa-weixin"></i></a>
                                <a href="/auth/github?next="><i class="fa fa-github"></i></a>
                                <a href="/auth/renren?next="><i class="fa fa-renren"></i></a>
                            </div>
                            <div class="form-group error-msg">
                                <div class="alert alert-danger" role="alert"></div>
                            </div>
                        </form>
                    </div>


                    <div role="tabpanel" class="tab-pane" id="signup-form">
                        <form>
                            <div class="form-group">
                                <div class="input-group">
                                    <div class="input-group-addon">
                                        <i class="fa fa-envelope" style="margin:0;"></i>
                                    </div>
                                    <input type="email" v-model="register_email" class="form-control" placeholder="请输入邮箱">
                                </div>
                            </div>
                            <div class="form-group">
                                <div class="input-group">
                                    <div class="input-group-addon">
                                        <i class="fa fa-lock" style="margin:0;"></i>
                                    </div>
                                    <input type="password" v-model="register_passwd" class="form-control" placeholder="请输入密码">
                                </div>
                            </div>
                            <div class="form-inline">
                                <div class="form-group">
                                    <div class="input-group">
                                        <input type="text"  v-model='register_verify' class="form-control" placeholder="请输入验证码">
                                    </div>
                                </div>
                                <input class="btn verify-code"  type="button" @click="send_verify_email" :value="mes">
                                <!-- 提示 -->
                                <div class="help-block text-left">{{message}}</div>
                            </div>
                            <div class="form-group">
                                <input class="btn btn-primary" @click="register"  type="button" value="注册">
                            </div>
                            <div class="form-group agree-privacy">
                                注册表示您已经同意我们的<a href="privacy/index.html" target="_blank">隐私条款</a>
                            </div>
                            <div class="form-group widget-signup">
                                <span>快速注册</span>
                                <a href="/auth/qq?next="><i class="fa fa-qq"></i></a>
                                <a href="https://api.weibo.com/oauth2/authorize?client_id=2028331105&redirect_uri=http://127.0.0.1:8080/weibo_callback"><i class="fa fa-weibo"></i></a>
                                <a href="/auth/weixin?next="><i class="fa fa-weixin"></i></a>
                                <a href="/auth/github?next="><i class="fa fa-github"></i></a>
                                <a href="/auth/renren?next="><i class="fa fa-renren"></i></a>
                            </div>
                            <!-- <div class="form-group error-msg">
                                <div class="alert alert-danger" role="alert"></div>
                            </div> -->
                        </form>
                    </div>
                </div>

            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name:'LoginReg',
    data() {
        return {
            register_email:'', //注册时的 邮箱
            register_passwd:'', //注册时的 密码
            register_verify:'',  //注册时的 验证码
            login_email:'',
            login_passwd:'',
            is_register:false, //判断是否能够注册，只有验证码输入正确时才可以注册
            is_unregister:true, //校验失败为true，成功为false
            message:'', //操作错误时 表单的提示信息,
            login_message:'',
            mes:'发送验证码',
            uid:''
        }
    },
    methods: {
        send_verify_email(){

            this.axios.post('/userapp/verify_code', {'email':this.register_email}).then(res=>{
                // alert(res.data.msg)
                this.mes = res.data.msg
            }).catch(error=>{
                alert(res.data.msg)
            })
        },
        register(){
            const data = {
                'email': this.register_email,
                'password': this.register_passwd,
                'verify_code': this.register_verify
            }
            this.axios.post('/userapp/register', data).then(res=>{
                alert(res.data.msg)
            })
        },
        login(){
            const data = {
                'email': this.login_email,
                'password': this.login_passwd,
            }
            this.axios.post('/userapp/login', data).then(res=>{
                alert(res.data.msg)
                if (res.data.code == 200){
                    sessionStorage.setItem('token', res.data.token)
                    sessionStorage.setItem('email', this.login_email)
                    // sessionStorage.setItem('id', res.data.id)
                    this.$router.go(0)
                }
                
            }).catch(error=>{
                alert(error.response.data.msg)
            })
        }
    },
    created(){
        const uid = this.$route.params.uid
        

    },
    
}
</script>

<style>

</style>
