<template>
    <div class="v-header">
        <div class="content-wrapper">
            <div class="avatar">
                <img width="64" height="64" :src="seller.avatar" alt="">
            </div>
            <div class="content">
                <div class="title">
                    <span class="brand"></span>
                    <span class="name">{{seller.name}}</span>
                </div>
                <div class="description">
                    {{seller.description}}/{{seller.deliveryTime}}分钟送达
                </div>
                <div v-if="seller.supports" class="support">
                    <span class="icon" :class="getType(seller.supports[0].type)"></span>
                    <span class="text">{{seller.supports[0].description}}</span>
                </div>    
            </div>
            <div v-if="seller.supports" class="support-count" @click="showDetail">
                <span class="count">{{seller.supports.length}}个</span>
                <i class="icon-keyboard_arrow_right"></i>
            </div>
        </div>
        <div class="bulletin-wrapper" @click="showDetail">
            <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
            <i class="icon-keyboard_arrow_right"></i>
        </div>
        <div class="background">
            <img :src="seller.avatar" alt="" width="100%" height="100%">
        </div>
        <transition name="fade">
            <div v-show="detailShow" class="detail">
                <div class="detail-wrapper clearfix">
                    <div class="detail-main">
                        <h1 class="name">{{seller.name}}</h1>
                        <Star :size="48" :score="seller.score" class="star"></Star>
                        <div class="title">
                            <div class="line"></div>
                            <div class="text">商家优惠</div>
                            <div class="line"></div>
                        </div>
                        <ul v-if="seller.supports" class="supports">
                            <li class="support-item" v-for="(item,index) in seller.supports" :key="index">
                                <span class="icon" :class="getType(item.type)"></span>
                                <span class="text">{{item.description}}</span>
                            </li>
                        </ul>
                        <div class="notice">
                            <div class="line"></div>
                            <div class="text">商家公告</div>
                            <div class="line"></div>
                        </div>
                        <div class="bulletin">
                            <div class="content">
                                {{seller.bulletin}}
                            </div>
                        </div>
                    </div>                  
                </div>
                <div class="detail-close" @click="hideDetail">
                    <i class="icon-close"></i>
                </div>
            </div>
        </transition> 
    </div>
</template>

<script>
import Star from '../star/star'
    export default{
        name:'Header',
        data(){
            return{
                type:['decrease','discount','special','invoice','guarantee'],
                detailShow:false,
            }
        },
         props: {
                    seller: {
                        type:Object,
                    },  
                },
        components:{
            Star
        },
        methods:{
            getType(num){
                return this.type[num];
            },

            showDetail(){
                this.detailShow = true
            },

            hideDetail(){
                this.detailShow = false
            }
        },
    }
</script>

<style lang="stylus" rel="stylesheet/stylus" rel="stylesheet/stylus" scoped>
@import "../../common/stylus/mixin.styl"
    .v-header
        position relative
        color #fff
        background  rgba(7,17,27,0.5)
        overflow hidden
        .content-wrapper
            position relative
            padding 24px 12px 18px 24px
            font-size 0
            .avatar
                display inline-block
                vertical-align top
                img
                    border-radius 2px
            .content
                display inline-block
                margin-left 6px
                font-size 14px
                .title
                    margin 2px 0 8px 0
                    .brand
                        display inline-block
                        vertical-align top
                        width 30px
                        height 18px
                        bg-image ('brand')
                        background-size 30px 18px
                        background-repeat no-repeat
                    .name
                        margin-left 6px
                        font-size 16px
                        line-height 18px
                        font-weight bold
                .description
                    margin-bottom 10px
                    line-height 12px
                    font-size 12px
                .support
                    .icon
                        display inline-block
                        vertical-align top
                        width 12px
                        height 12px
                        margin-right 4px
                        background-size 12px 12px
                        background-repeat no-repeat
                        &.decrease
                            bg-image('decrease_1')
                        &.discount
                            bg-image('discount_1')
                        &.guarantee
                            bg-image('guarantee_1')
                        &.invoice
                            bg-image('invoice_1')
                        &.special
                            bg-image('special_1')
                    .text
                        vertical-align top
                        line-height 12px
                        font-size 10px
            .support-count
                position absolute 
                bottom 14px
                right 12px
                padding 0 8px
                height 24px
                line-height 24px
                border-radius 14px
                background rgba(0,0,0,0.2)
                text-align center
                .count
                    vertical-align top
                    font-size 10px
                .icon-keyboard_arrow_right
                    font-size 10px
                    line-height 24px
                    margin-left 2px
        .bulletin-wrapper
            position relative
            height 28px
            line-height 28px
            padding 0 22px 0 12px
            white-space nowrap
            overflow hidden
            text-overflow ellipsis
            background rgba(7,17,27,0.2)
            .bulletin-title
                vertical-align top
                margin-top 8px
                display inline-block
                width 22px
                height 12px
                bg-image('bulletin')
                background-size 22px 12px
                background-repeat no-repeat
            .bulletin-text
                vertical-align top
                font-size 10px
                font-weight 200
                margin 0 4px
            .icon-keyboard_arrow_right
                position absolute
                right 12px
                top 8px
                font-size 10px
        .background
            position absolute
            top 0
            left 0
            height 100%
            width 100%
            z-index -1
            filter:blur(10px)
        .detail
            position fixed
            top 0 
            left 0
            z-index 10
            width 100%
            height  100%
            overflow auto 
            background rgba(7,17,27,0.8)
            &.fade-enter-active,.fade-leave-active
                transition all 0.5s
                background rgba(7,17,27,0.8)
            &.fade-enter,.fade-leave-to
                transition all 0.5s
                background rgba(7,17,27,0)
            .detail-wrapper
                width:100%
                min-height 100%
                .detail-main
                    margin-top 64px
                    padding-bottom 64px
                    .name
                        text-align center
                        line-height 16px
                        color:#ffff
                        font-size 16px
                        font-weight 700
                    .star
                        text-align center
                        margin-top 18px
                        height 24px
                        line-height 24px
                    .title,.notice
                        display flex
                        width 80%
                        margin 24px auto 28px auto
                        .line
                            flex 1
                            position relative
                            top -6px
                            border-bottom 1px solid rgba(255,255,255,0.2)
                        .text
                            font-size 14px
                            font-weight 700
                            padding 0 12px
                    .supports
                        width 80%
                        margin 0 auto
                        .support-item
                            padding 0 12px
                            margin-bottom 12px
                            font-size 0
                            &:last-child
                                margin-bottom 0
                            .icon
                                display inline-block
                                width 16px
                                height 16px
                                vertical-align top
                                margin-right 6px
                                background-size 16px 16px
                                background-repeat no-repeat
                                &.decrease
                                    bg-image('decrease_2')
                                &.discount
                                    bg-image('discount_2')
                                &.guarantee
                                    bg-image('guarantee_2')
                                &.invoice
                                    bg-image('invoice_2')
                                &.special
                                    bg-image('special_2')
                            .text
                                vertical-align top
                                display inline-block
                                line-height 16px
                                height 16px
                                font-size 12px
                                font-weight 200  
                    .bulletin
                        margin 0 auto
                        width 80%
                        .content
                            padding 0 12px
                            line-height 24px
                            font-size 12px
            .detail-close
                    position relative
                    width 32px
                    height 32px
                    margin -64px auto 0 auto
                    clear both
                    font-size 32px 
                        
</style>

