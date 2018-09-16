<template>
<div id="sp_container" class="container" :style="clientH">
    <div class="f_video" @click="chickLogin">
        <div class="content">
            <div class="ad" v-if="isAd">
                <img class="ad_img" src="../image/ad.png"/>
                <img class="ad_close" @click="closeAd" src="../image/close1.png" />
            </div>
            <div class="follow_box" flex="dir:left cross:center main:left">
                <img src="../image/zan.png"/>
                <div flex="dir:top">
                    <span class="store_name">云南翡翠世家啦啦啦</span>
                    <span class="view_number">{{viewCount}}人观看</span>
                </div>
                <span class="follow_btn" :class="{active:isFollow}">
                    {{isFollow ? '已关注':'关注'}}
                </span>
            </div>
            <div class="barrage">
                <div class="_rc">
                    <div class="_rc_l" v-for="(item,index) in barrageList" :key="index">
                        <div>{{item}}</div>
                    </div>
                </div>
            </div>
            <div class="send_box">
                <input placeholder="跟主播说点什么吧" type="text" v-model="sendMsg">
                <span class="send_btn">发送</span>
            </div>
            <div class="zan_box" flex="dir:top cross:center">
                <span>{{likeCount}}</span>
                <img src="../image/zan.png" />
            </div>
            <div id="more" class="more" @click="lookMoreRoom" flex="dir:left cross:center main:center">
                <img src="../image/line.png"/>
                <span>更多源头工厂正在直播</span>
                <img src="../image/line.png"/>
            </div>
        </div>
        <div class="player_box" :style="clientH">
            <div class="prism-player" id="J_prismPlayer" style="position: absolute" :style="clientH"></div>
        </div>
        
    </div>
    <div class="room_list" flex="dir:left">
        <div class="room" v-for="(r,i) in liveRooms" :key="i">
            <img class="r_img" :src="r.image"/>
            <div class="live_box" flex="dir:left cross:center">
                <img class="live_img" src="../image/live.png"/>
                <span class="r_number">3213观看</span>
            </div>
            <img class="zan_img" src="../image/zan1.png"/>
            <span class="name">{{r.name}}</span>
        </div>
    </div>
    <div class="download_btn" v-if="downloadBtn">下载翡标奢品APP，享受直播购物盛典</div>
    <Login v-if="showLogin"></Login>
    <DownLoad v-if="showDownLoad"></DownLoad>
</div>

</template>
<script>
import { MessageBox  } from 'mint-ui';
import $ from 'jquery';
import Login from './Login';
import DownLoad from './DownLoad';
import store from '../store';
import {mapState, mapMutations} from "vuex";
export default {
    components: {
        Login,
        DownLoad
    },
    data(){
        return{
            clientH: {
                height: document.documentElement.clientHeight + 'px',
            },
            liveRooms: [
                {
                    bigImage:"https://img.dz8.cn/activity/2018-09/425395d932cde3b3277cf008febb37d5.png",
                    description:"瑞丽翡翠公盘 原石行家带你选料",
                    image:"https://img.dz8.cn/activity/2018-09/d3aaef35ba66b6215e6012e578d15775.png",
                    liveStatus:true,
                    name:"瑞丽公盘·原石专场",
                    recordCode:"dvFd5dsKGZcJjL2LOrUGx0",
                    roomCode:"170933",
                    shareUserCount:null,
                    sort:null,
                    startTimeLong:null,
                    type:1,
                    viewCount:"3862观看"
                },
                {
                    bigImage:"https://img.dz8.cn/activity/2018-09/425395d932cde3b3277cf008febb37d5.png",
                    description:"瑞丽翡翠公盘 原石行家带你选料",
                    image:"https://img.dz8.cn/activity/2018-09/d3aaef35ba66b6215e6012e578d15775.png",
                    liveStatus:true,
                    name:"瑞丽公盘·原石专场",
                    recordCode:"dvFd5dsKGZcJjL2LOrUGx0",
                    roomCode:"170933",
                    shareUserCount:null,
                    sort:null,
                    startTimeLong:null,
                    type:1,
                    viewCount:"3862观看"
                },
                {
                    bigImage:"https://img.dz8.cn/activity/2018-09/425395d932cde3b3277cf008febb37d5.png",
                    description:"瑞丽翡翠公盘 原石行家带你选料",
                    image:"https://img.dz8.cn/activity/2018-09/d3aaef35ba66b6215e6012e578d15775.png",
                    liveStatus:true,
                    name:"瑞丽公盘·原石专场",
                    recordCode:"dvFd5dsKGZcJjL2LOrUGx0",
                    roomCode:"170933",
                    shareUserCount:null,
                    sort:null,
                    startTimeLong:null,
                    type:1,
                    viewCount:"3862观看"
                }
            ],
            barrageList: [
                '欢迎xxx进入房间1',
                '欢迎xxx进入房间2',
                '欢迎xxx进入房间3',
                '欢迎xxx进入房间4',
                '欢迎xxx进入房间5',
                '欢迎xxx进入房间6',
            ],
            viewCount: 5,//观看人数
            likeCount: 123,//点赞次数
            sendMsg: '',//要发送的消息
            isFollow: false,//是否已关注
            isAd: true,//是否显示广告
            downloadBtn: false, //显示底部下载按钮
        }
    },
    computed: {
        ...mapState({
            isLogin:({ common }) => common.isLogin,
            showLogin:({ common }) => common.showLogin,
            showDownLoad:({common}) => common.showDownLoad
        })
    },
    mounted: function() {
        //初始化video
        this.initVideo();
        //监听滚动出现下载按钮
        this.$el.addEventListener('scroll', this.handleScroll);
        //循环弹窗
        this.downloadBox();
    },
    methods: {
        ...mapMutations({
            toggleShowLogin: "SHOW_LOGIN",
            toggleshowDownLoad: "SHOW_DOWNLOAD"
        }),
        handleScroll: function(){
            var scrollTop = this.$el.scrollTop;
            if(scrollTop > 180){
                this.downloadBtn = true;
            }else{
                this.downloadBtn = false;
            }
        },
        downloadBox: function(){
            //循环弹窗
            let that = this;
            let timer = setInterval(()=>{
                if(!that.showLogin && !that.showDownLoad){
                    that.toggleshowDownLoad(true);
                }
            },60000);
        },
        getNewMsg: function(){
            if(this.barrageList.length>10){
                this.barrageList.shift();
            }
            this.barrageList.push('欢迎xxx进入房间'+this.barrageList.length);
        },
        //初始化视频
        initVideo: function(){
            var self = this;
            var videoid = '223';
            var playauther = '123';
            var player = new Aliplayer({
                id: 'J_prismPlayer',
                width: '100%',
                height: '100%',
                autoplay: false,
                vid: videoid,
                playauther: playauther,
                isLive: false,
                playsinline: true,
                preload: false,
                controlBarVisibility: 'hover'
            });
            // setInterval(function(){
            //     self.getNewMsg();
            // },5000);
        },
        //点击查看更多房间
        lookMoreRoom: function(){
            //置顶，更多源头工厂正在直播
            var clientH = document.documentElement.clientHeight;
            var moreH = this.$el.querySelector('#more').scrollHeight;
            this.$nextTick(()=>{
                this.$el.scrollTop = clientH -moreH;
            })
        },
        //检查是否登录，是否弹出登录框
        chickLogin: function(){
            if(!this.isLogin){
                this.toggleShowLogin();
            }
        },
        //关闭广告
        closeAd: function(){
            this.isAd = false;
        }
    },
    watch: {
        barrageList(){
            this.$nextTick(()=>{
                var container = this.$el.querySelector('._rc');
                container.scrollTop = container.scrollHeight
            })
        }
    }
}
</script>
<style lang="less" scoped>
.container{
    overflow-x: hidden;
}
.f_video{
    position: relative;
    overflow-x: hidden;
    .content{
        background: #eee;
        position: absolute;
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
        z-index: 101;
        font-size: 0.32rem;
        .ad{
            position: relative;
            .ad_img{
                width: 100%;
                max-height: 3rem;
            }
            .ad_close{
                position: absolute;
                top: 50%;
                right: .2rem;
                transform: translateY(-50%);
                width: .48rem;
            }
        }
        .follow_box{
            color: #fff;
            background: fade(hsl(0, 0, 0), 10%);
            padding: .1rem .2rem;
            display: inline-flex;
            border-radius: 0 .8rem .8rem 0;
            margin-top: .1rem;
            img{
                width:.6rem;
                height: .6rem;
                border-radius: .3rem;
                background: red;
                margin-right: .1rem;
            }
            .store_name{
                font-size: .24rem;
            }
            .view_number{
                font-size: .2rem;
                margin-top: .05rem;
            }
            .follow_btn{
                margin-left: .1rem;
                background: #06823E;
                border-radius: .08rem;
                width: 1rem;
                height: .48rem;
                line-height: .48rem;
                text-align: center;
                font-size: .24rem;
            }
            .follow_btn.active{
                background: #9CA8A1;
            }
        }
        .barrage{
            position: absolute;
            height: 2.42rem;
            width: 5.8rem;
            overflow: hidden;
            padding: 0 .2rem;
            bottom: 2.44rem;
            left: 0;
            z-index: 1;
            ._rc{
                height: 100%;
                width: 6.2rem;
                overflow-x: auto;
                padding-right: .6rem;
                ._rc_l{
                    margin-top: .1rem;
                    font-size: .32rem;
                    max-width: 100%;
                    div{
                        display: inline-block;
                        background: #fff;
                        border-radius: .25rem;
                        padding: .1rem .2rem;
                        max-width: 100%;
                    }
                }
            }
        }
        .send_box{
            position: absolute;
            width: 5.4rem;
            height: .8rem;
            line-height: .8rem;
            left: .2rem;
            bottom: 1.34rem;
            z-index: 1;
            border-radius: .4rem;
            padding-right: 1.32rem;
            background-color: rgba(0,0,0,.4);
            input{
                width: 4.08rem;
                box-sizing: border-box;
                border-radius: .4rem;
                padding: 0 .24rem;
                border: 0 none;
                outline: 0 none;
                color: #fff;
                font-size: .32rem;
                background-color: transparent;
            }
            input::-webkit-input-placeholder{
                color: #fff;
            }
            .send_btn{
                width: 1.12rem;
                height: .6rem;
                line-height: .6rem;
                color: #888;
                background-color: #eee;
                border-radius: .3rem;
                text-align: center;
                position: absolute;
                font-size: .3rem;
                top: .1rem;
                right: .1rem;
                bottom: .1rem;
            }   
        }
        .zan_box{
            position: absolute;
            right: 0.12rem;
            bottom: 1.34rem;
            z-index: 10;
            img{
                width: .8rem;
                height: .8rem;
                z-index: 100;
            }
            span{
                background: #f43530;
                padding: 0.05rem .1rem;
                color: #fff;
                font-size: .24rem;
                border: .02rem solid #fff;
                border-radius: .15rem;
            }
        }
        .more{
            height: 1.04rem;
            font-size: .36rem;
            color: #00c17b;
            background-color: #fff;
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            img{
                width: .22rem;
            }
            span{
                margin: 0 .4rem;
            }
        }
    }
    .player_box{
        position: relative;
        width: 100%;
    }
}
.room_list{
    position: relative;
    font-size: 0.32rem;
    flex-wrap: wrap;
    padding-bottom: 1rem;
    .room{
        position: relative;
        width: 50%;
        margin-bottom: .1rem;
        height: 3.7rem;
        .r_img{
            width: 100%;
            height: 100%;
        }
        .live_box{
            position: absolute;
            left: .2rem;
            top: .2rem;
            .live_img{
                width: 1rem;
            }
            .r_number{
                background: fade(hsl(231, 231, 231), 20%);
                border-radius: .4rem;
                color: #fff;
                font-size: .16rem;
                margin-left: .1rem;
            }
        }
        
        .zan_img{
            position: absolute;
            right: .1rem;
            bottom: .2rem;
            width: .58rem;
            height: auto;
            z-index: 2;
        }
        .name{
            position: absolute;
            overflow: hidden;
            -webkit-text-overflow: ellipsis;
            -ms-text-overflow: ellipsis;
            text-overflow: ellipsis;
            white-space: nowrap;
            width: 2.24rem;
            height: .4rem;
            line-height: .4rem;
            font-size: .28rem;
            color: #fff;
            left: .2rem;
            bottom: .16rem;
        }
    }
    .room:nth-child(odd){
        padding-right: .05rem;
    }
    .room:nth-child(2n){
        padding-left: .05rem;
    }
}
.download_btn{
    position: absolute;
    width: 100%;
    left: 0;
    bottom: 0;
    height: 1rem;
    background: #06823E;
    color: #fff;
    line-height: 1rem;
    text-align: center;
    z-index: 110;
    font-size: .36rem;
}

</style>


