<template>
    <div class="mask_box">
        <div class="mask"></div>
        <div class="content" flex="dir:top cross:center">
            <img  @click="toggleShowLogin" class="close" src="../image/close.png"/>
            <div class="title">请登录</div>
            <div class="tel" flex="dir:left cross:center">
                <span>手机号</span>
                <input v-model="mobile" flex-box="1" type="tel" maxlength="11" placeholder="请输入手机号码"/>
            </div>
            <div class="code" v-if="ncSuccess">
                <input v-model="code" type="text" placeholder="请输入手机验证码"/>
                <span class="count_down" v-if="isSend">{{totalTime}}s后重发</span>
                <span @click="getCode" v-else>获取验证码</span>
            </div>
            <div class="opt">
                <div id="nc"></div>
            </div>
            <div class="login_btn" v-if="ncSuccess" @click="goLogin">登录</div>
        </div>
    </div>
</template>
<script>
import { Toast } from 'mint-ui';
import Tool from "../utils/Tool";
import {mapState, mapMutations} from "vuex";

export default {
    data(){
        return{
            mobile: '',//手机号码
            code: '',//短信验证码
            ncSuccess: false,//滑动组件是否成功
            isSend: false,//是否发送短信验证码
            totalTime: 60,//倒计时，默认60秒
        }
    },
    computed: {
        ...mapState({
            isLogin:({ common }) => common.isLogin,
            showLogin:({ common }) => common.showLogin,
        })
    },
    mounted: function(){
        this.initnc();
    },
    methods: {
        ...mapMutations({
            toggleShowLogin: "SHOW_LOGIN",
            setLogin: "IS_LOGIN"
        }),
        //初始化滑块
        initnc: function(){
            var self = this;
            var nc_token = ["CF_APP_1", (new Date()).getTime(), Math.random()].join(':');
            var nc=NoCaptcha.init({
                renderTo: '#nc',
                appkey: 'CF_APP_1', 
                scene: 'register',
                token: nc_token,
                trans: {"key1": "code0"},
                elementID: ["usernameID"],
                is_Opt: 0,
                language: "cn",
                timeout: 10000,
                retryTimes: 5,
                errorTimes: 5,
                inline:false,
                apimap: {
                    // 'analyze': '//a.com/nocaptcha/analyze.jsonp',
                    // 'uab_Url': '//aeu.alicdn.com/js/uac/909.js',
                },
                bannerHidden:false,
                initHidden:false,
                callback: function (data) {
                    self.ncSuccess = true;
                    window.console && console.log(nc_token)
                    window.console && console.log(data.csessionid)
                    window.console && console.log(data.sig)
                },
                error: function (s) {
                }
            });
            NoCaptcha.setEnabled(true);
            nc.reset();//请务必确保这里调用一次reset()方法
            NoCaptcha.upLang('cn', {
                'LOADING':"加载中...",//加载
                'SLIDER_LABEL': "请按住滑块，滑到最右端",//等待滑动
                'CHECK_Y':"验证成功",//通过
                'ERROR_TITLE':"非常抱歉，这出错了...",//拦截
                'CHECK_N':"验证未通过", //准备唤醒二次验证
                'OVERLAY_INFORM':"经检测你当前操作环境存在风险，请输入验证码",//二次验证
                'TIPS_TITLE':"验证码错误，请重新输入"//验证码输错时的提示
            });
        },
        //获取验证码
        getCode: function(){
            if(this.mobile.length !== 11){
                Toast('请输入11位手机号码');
                return;
            }
            this.isSend = true;
            let clock = setInterval(()=>{
                this.totalTime -- ;
                if(this.totalTime < 0){
                    clearInterval(clock);
                    this.isSend = false;
                    this.totalTime = 60;
                }
            },1000);
            Tool.get('goods/buyCarCon',{},(data)=>{
                console.log(data)
            },(err)=>{})
        },
        goLogin: function(){
            if(!this.ncSuccess){
                Toast('滑动验证未通过');
                return;
            }
            if(this.mobile.length !== 11){
                Toast('请输入11位手机号码');
                return;
            }
            if(!this.code){
                Toast('请输入手机验证码');
                return;
            }
            
            Toast('校验验证码');
            Toast('登录成功');
            //关闭登录框
            this.toggleShowLogin();
            //设置登录状态
            this.setLogin(true);
        },
    },
    props: {
        // close: {
        //     type: Function,
        //     defalult: function(){
        //         return;
        //     }
        // }
    }
}
</script>
<style lang="less" scoped>
.mask_box{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1100;
    color: #333;
    .mask{
        width: 100%;
        height: 100%;
        position: fixed;
        opacity: 0.5;
        background: #000;
    }
    .content{
        position: fixed;
        top: 50%;
        left: 50%;
        -webkit-transform: translate3d(-50%, -50%, 0);
        transform: translate3d(-50%, -50%, 0);
        background-color: #fff;
        width: 85%;
        border-radius: .1rem;
        font-size: .32rem;
        -webkit-user-select: none;
        overflow: hidden;
        -webkit-backface-visibility: hidden;
        backface-visibility: hidden;
        -webkit-transition: .2s;
        transition: .2s;
        padding: .3rem .2rem .47rem;
        input{
            height: .84rem;
            border: 0;
            background: #F0F2F5;
            padding-left: .2rem;
            border-radius: .08rem;
        }
        input:focus{
            border:0;
        }
        .title{
            font-size: .36rem;
        }
        .close{
            position: absolute;
            width: 0.54rem;
            right: 0.1rem;
            top: 0.1rem;
        }
        .tel{
            width: 100%;
            margin-top: .47rem;
            input{
                margin-left: .2rem;
            }
        }
        .code{
            position: relative;
            width: 100%;
            margin-top: .2rem;
            input{
                width: 100%;
            }
            span{
                position: absolute;
                right: .1rem;
                top: 50%;
                transform: translateY(-50%);
                color: #fff;
                background: #06823E;
                padding: .1rem;
                border-radius: .1rem;
            }
            .count_down{
                background: #D1D3D5;
                color: #999;
            }
        }
        .opt{
            margin-top: .2rem;
            width: 100%;
        }
        .login_btn{
            width:100%;
            height: .9rem;
            line-height: .9rem;
            text-align: center;
            border-radius: .08rem;
            margin-top: .4rem;
            background: #06823E;
            color: #fff;
            font-size: .36rem;
        }
    }
}

</style>
<style lang="less">
#nc .stage1{
    padding: 0;
    height: .9rem;
}
#nc .stage1 .icon{
    display: none;
}
#nc .stage1 .slider{
    left: 0;
    right: 0;
    box-shadow: 0 0 1px #999;
    height: .9rem;
    *{
        height: .9rem;
        line-height: .9rem;
    } 
}
#nc .stage1 .bg-green{
    background-color: #6E9FE8;
    font-size: .32rem;
}
</style>


