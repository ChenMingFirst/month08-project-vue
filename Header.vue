<template>
    <div>
        <nav class="navbar navbar-default navbar-fixed-top header">
            <div class="container">
                <div class="navbar-header">
                    <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#header-navbar-collapse" aria-expanded="false">
                        <span class="sr-only">实验楼</span>
                        <span class="icon-bar"></span>
                        <span class="icon-bar"></span>
                        <span class="icon-bar"></span>
                    </button>
                    <a class="navbar-brand" href="/">
                        <img src="static/img/logo_03.png">
                    </a>
                </div>
                <div class="collapse navbar-collapse" id="header-navbar-collapse">
                    <ul class="nav navbar-nav">
                        <li class="dropdown ">
                            <a href="javascript:void(0);" class="dropdown-toggle" data-toggle="dropdown">
                                课程
                                <span class="caret"></span>
                            </a>
                            <ul class="dropdown-menu">
                                <li><a class="" href="/courses" >全部课程</a></li>
                                <li><a class="" href="courses/index.html?status=preview" >即将上线</a></li>
                                <li><a class="" href="developer/index.html">开发者</a></li>
                            </ul>
                        </li>
                        <li class="">
                            <a href="paths/index.html">路径</a>
                        </li>
                        <li class="">
                            <a href="questions/index.html">讨论区</a>
                        </li>
                        <li class=" bootcamp new-nav logo-1111">
                            <a href="bootcamp/index.html">训练营</a>
                            
                        </li>
                        <li class=" new-nav logo-1111">
                            <a href="vip/index.html">会员</a>
                            
                        </li>
                        
                    </ul>

                    <div class="navbar-right btns" v-if="email">
                        <p class="btn btn-default navbar-btn sign-in">欢迎您：<em>{{email}}</em></p>
                        <a href="http://127.0.0.1:8080/pim" class="btn btn-default navbar-btn sign-up">个人信息</a>
                        <a href="javascript:;" class="btn btn-default navbar-btn sign-up" @click="quit">退出</a>
                    </div>
				
                    <div class="navbar-right btns" v-else>
                        <a class="btn btn-default navbar-btn sign-in" data-sign="signin" href="#sign-modal" data-toggle="modal">登录</a>
                        <a class="btn btn-default navbar-btn sign-up" data-sign="signup" href="#sign-modal" data-toggle="modal">注册</a>
                    </div>
                    

                    <form class="navbar-form navbar-right" action="search" method="get" role="search">
                        <div class="form-group">
                            <input type="text" class="form-control" name="search" autocomplete="off" placeholder="搜索 课程/问答">
                        </div>
                    </form>
                </div>
            </div>
        </nav>
    </div>
</template>

<script>

export default {
    name:'Header',
    data(){
        return {
            email: sessionStorage.getItem('email')
        }
    },
    methods:{
        quit(){
            sessionStorage.removeItem('email')
            sessionStorage.removeItem('token')
            this.$router.push({'path': '/index'})
        }
    }

}
</script>

<style>

</style>
