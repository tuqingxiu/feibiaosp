<template>
    <div class="mask_box">
        <div class="mask"></div>
        <div class="content" flex="dir:top cross:center">
            <img  @click="callback" class="close" src="../image/close.png"/>
            <div class="title">请登录</div>
            <div class="tel">
                <span>手机号</span>
                <input type="tel" placeholder="请输入手机号码"/>
            </div>
            <div class="code">
                <input type="text" placeholder="请输入手机验证码"/>
                <span>获取验证码</span>
            </div>
            <div class="opt">
                <div id="nc"></div>
            </div>
            <div class="login_btn">登录</div>
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return{

        }
    },
    mounted: function(){
        var nc_token = ["CF_APP_1", (new Date()).getTime(), Math.random()].join(':');
        var nc=NoCaptcha.init({
            renderTo: '#nc',
            appkey: 'CF_APP_1', 
            scene: 'register',
            token: nc_token,
            trans: {"key1": "code200"},
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
    props: {
        callback: {
            type: Function,
            defalult: function(){
                return;
            }
        }
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
    z-index: 2000;
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
        padding: .3rem .2rem;
        input{
            height: .84rem;
            border: 0;
            background: #f5f5f5;
            padding-left: .2rem;
            border-radius: .08rem;
        }
        .title{
            font-size: .36rem;
        }
        .close{
            position: absolute;
            width: 0.25rem;
            right: 0.2rem;
            top: .3rem;
        }
        .tel{
            width: 100%;
            margin-top: .47rem;
            input{
                width: 75%;
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
        }
        .opt{
            margin-top: .2rem;
            width: 100%;
            height: .9rem;
        }
    }
}
</style>

