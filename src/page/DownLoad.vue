<template>
    <div class="mask_box">
        <div class="mask"></div>
        <div class="content" flex="dir:top cross:center">
            <div class="text" flex="dir:top cross:center">
                <span>下载翡标奢品APP</span>
                <span>享受直播购物盛典</span>
            </div>
            <div class="btns" flex="dir:left cross:center main:center">
                <span flex-box="1" class="cancel" @click="cancel">取消</span>
                <span flex-box="1" class="download">下载</span>
            </div>
        </div>
    </div>
</template>
<script>
import {mapState, mapMutations} from "vuex";
export default {
    data(){
        return{

        }
    },
    computed: {
    },
    methods: {
        ...mapMutations({
            toggleshowDownLoad: "SHOW_DOWNLOAD"
        }),
        cancel: function(){
            this.toggleshowDownLoad(false);
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
    z-index: 1200;
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
        font-size: .36rem;
        -webkit-user-select: none;
        overflow: hidden;
        -webkit-backface-visibility: hidden;
        backface-visibility: hidden;
        -webkit-transition: .2s;
        transition: .2s;
        .text{
            margin: .6rem 0;
            span{
                line-height: .6rem;
            }
        }
        .btns{
            width: 100%;
            height: .9rem;
            text-align: center;
            line-height: .9rem;
            span{
                height: 100%;
            }
            .cancel{
                border-top: 1px solid #979797;
            }
            .download{
                background: #06823E;
                color: #fff;
            }
        }
    }
}

</style>