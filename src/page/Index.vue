<template>
<div>
    <div class="f_video">
        <div class="content">
            <div class="follow_box" flex="dir:left"><span>缅甸翡翠代购</span><span>关注</span></div>
            <span class="user_number">3232人观看</span>
            <div class="barrage">
                <div class="_rc">
                    <div v-for="(item,index) in barrageList" :key="index">{{item}}</div>
                </div>
            </div>
            <div class="send_box">
                <input placeholder="跟主播说点什么吧" type="text">
                <span class="send_btn">发送</span>
            </div>
        </div>
        <div class="prism-player" id="J_prismPlayer" style="position: absolute" :style="playerStyle"></div>
    </div>
    <div class="v_list"></div>
</div>
    
</template>
<script>
export default {
    data(){
        return{
            playerStyle: {
                height: document.documentElement.clientHeight + 'px',
            },
            barrageList: [
                '欢迎xxx进入房间1',
                '欢迎xxx进入房间2',
                '欢迎xxx进入房间3',
                '欢迎xxx进入房间4',
                '欢迎xxx进入房间5',
                '欢迎xxx进入房间6',
            ]
        }
    },
    methods: {
        getNewMsg: function(){
            if(this.barrageList.length>10){
                this.barrageList.shift();
            }
            this.barrageList.push('欢迎xxx进入房间'+this.barrageList.length);
        }
    },
    watch: {
        barrageList(){
            this.$nextTick(()=>{
                var container = this.$el.querySelector('._rc');
                container.scrollTop = container.scrollHeight
            })
        }
    },
    mounted: function() {
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
        setInterval(function(){
            self.getNewMsg();
        },5000);
    }
}
</script>
<style lang="less" scoped>
.content{
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    z-index: 101;
    font-size: 0.32rem;
    .follow_box{
        background: #fff;
    }
    .user_number{
        background: #fff;
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
            div{
                background: #fff;
                margin-top: .1rem;
                line-height: .5rem;
                font-size: .32rem;
                border-radius: .25rem;
                max-width: 100%;
                padding: 0 .2rem;
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
    }
}
</style>


