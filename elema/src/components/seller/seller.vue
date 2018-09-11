<template>
    <div class="seller" ref="seller">
        <div class="seller-content">
            <div class="overview">
                <h1 class="name">{{seller.name}}</h1>
                <div class="desc border-1px">
                    <star :size="36" :score="seller.score"></star>
                    <span class="text">({{seller.ratingCount}})</span>
                    <span class="text">月售{{seller.sellCount}}单</span>
                </div>
                <ul class="remark">
                    <li class="block">
                        <h2 class="title">起送价</h2>
                        <div class="content">
                            <span class="stress">{{seller.minPrice}}</span>元
                        </div>
                    </li>
                    <li class="block">
                        <h2 class="title">商家配送</h2>
                        <div class="content">
                            <span class="stress">{{seller.deliveryPrice}}</span>元
                        </div>
                    </li>
                    <li class="block">
                        <h2 class="title">平均配送时间</h2>
                        <div class="content">
                            <span class="stress">{{seller.deliveryTime}}</span>分钟
                        </div>
                    </li>
                </ul>
                <div class="favorite" @click="toggleFavorite">
                    <span class="icon-favorite" :class="{'active':favorite}"></span>
                    <span class="text">{{favoriteText}}</span>
                </div>
            </div>
            <split></split>
            <div class="bulletin">
                <h1 class="title">公告与活动</h1>
                <div class="content-wrapper">
                    <p class="content">{{seller.bulletin}}</p>
                </div>
                <ul v-if="seller.supports" class="supports">
                    <li v-for="(item,index) in seller.supports" :key="index" class="support-item border-1px">
                        <span class="icon" :class="getType(seller.supports[index].type)"></span>
                        <span class="text">{{seller.supports[index].description}}</span>
                    </li>
                </ul>
            </div>
            <split></split>
            <div class="pics">
                <h1 class="title">商家实景</h1>
                <div class="pic-wrapper">
                    <ul class="pic-list">
                        <li class="pic-item" v-for="(pic,index) in seller.pics" :key="index">
                            <img :src="pic" width="120" height="90">
                        </li>
                    </ul>
                </div>
            </div>
            <split></split>
            <div class="info">
                <h1 class="title">商家信息</h1>
                <ul>
                    <li class="info-item" v-for="(info,index) in seller.infos" :key="index">{{info}}</li>
                </ul>
            </div>
        </div>
        <shopcar ref="shopcart" :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice" :selectFoods="selectFoods"></shopcar>
    </div>
</template>
<script>
import shopcar from '../shopcar/shopcar'
import BScroll from 'better-scroll'
import star from '../star/star'
import split from '../split/split'
import {saveToLocal} from '../../common/js/store'
export default{
    name:"seller",
    data(){
        return{
            type:['decrease','discount','special','invoice','guarantee'],
            favorite:true,
            goods:[]
        }
    },
    created(){
        this.$axios.get('/api/goods')
                    .then(res => {
                        this.goods = res.data.data;    
                    })
                    .catch(err => {
                        console.log(err);
                    })
    },
    props: {
        seller:{
            type:Object
        }
    },
    components:{
        star,
        split,
        shopcar
    },
    computed:{
        selectFoods() {
            let foodss = []
           this.goods.forEach( (good) => {
               good.foods.forEach( (food) => {
                   if(food.count){
                       foodss.push(food)
                   }
               })
           }) 
           return foodss
        },
        favoriteText(){
           return this.favorite ? '已收藏' : '收藏'
        }
    },
    methods:{
        getType(num){
            return this.type[num];
        },
       toggleFavorite(){
           this.favorite = !this.favorite;
           saveToLocal(this.seller.id,'favorite',this.favorite)
       }
    },
}
</script>
<style lang="stylus" rel="stylesheet/stylus" scoped>
    @import "../../common/stylus/mixin";
        .seller-content
            position:absolute
            top 174px
            left 0
            bottom 0
            width 100%
            .overview
                position relative
                padding 18px
                .name
                    margin-bottom  8px
                    line-height 14px
                    font-size 14px
                    color rgb(7,17,27)
                .desc
                    padding-bottom 18px
                    border-1px(rgba(7,17,27,0.1))
                    font-size 0
                    .star
                        display inline-block
                        vertical-align top
                        margin-right 8px
                    .text
                        display inline-block
                        vertical-align top
                        margin-right 12px
                        line-height 18px
                        color rgb(77,85,93)
                        font-size 10px
                .remark
                    display flex
                    margin-top 18px
                    .block
                        flex 1
                        text-align center
                        border-right 1px solid rgba(7,17,27,0.1);
                        &.block:last-child
                            border-right none
                        .title
                            line-height 10px
                            margin-bottom 4px
                            font-size 10px
                            color rgb(147,153,159)
                        .content
                            font-size 10px
                            .stress
                                line-height 24px
                                font-weight 200
                                font-size 24px
                                color rgb(7,17,27)
                .favorite
                    position absolute
                    width 50px
                    top 18px
                    right 11px
                    font-size 0
                    text-align center
                    .icon-favorite
                        display block
                        line-height 24px
                        margin-bottom 4px
                        font-size 24px
                        color rgb(147,153,159)
                        &.active
                            color rgb(240,20,20)
                    .text
                        display block
                        line-height 10px
                        font-size 10px
                        color rgb(77,85,93)
            .bulletin
                padding 18px 18px 0 18px
                .title
                    line-height 14px
                    font-size 14px
                    color rgb(7,17,27)
                .content-wrapper
                    padding 8px 12px 16px 12px
                    line-height 24px
                    color rgb(240,20,20)
                    font-size 12px
                    font-weight 200
                .supports
                    .support-item
                        border-top 1px solid rgba(7,17,27,0.1)
                        padding 16px 12px
                        font-size 0
                        .icon
                            margin-right 6px
                            width 16px
                            height 16px
                            display inline-block
                            vertical-align top
                            background-size 16px 16px
                            background-repeat no-repeat
                            &.decrease
                                bg-image('decrease_4')
                            &.discount
                                bg-image('discount_4')
                            &.guarantee
                                bg-image('guarantee_4')
                            &.invoice
                                bg-image('invoice_4')
                            &.special
                                bg-image('special_4')
                        .text
                            display inline-block
                            vertical-align top
                            line-height  16px
                            color  rgb(7,17,27)
                            font-size 12px
                            font-weight 200
            
            
            
            
            
            
            
            .pics
                padding 18px
                .title
                    margin-bottom  12px
                    line-height 14px
                    font-size 14px
                    color rgb(7,17,27)
                .pic-wrapper
                    width 100%
                    overflow auto
                    white-space nowrap
                    .pic-item
                        display inline-block
                        margin-right 6px
                        width 120px
                        height 90px;
                        &:last-child
                            margin-right 0
            .info
                padding 18px 18px 0 18px
                .title
                    margin-bottom  12px
                    line-height 14px
                    font-size 14px
                    color rgb(7,17,27)
                .info-item
                    padding 16px 12px
                    border-top 1px solid rgba(7,17,27,0.1)
                    line-height 16px
                    font-weight 200
                    color rgb(7,17,27)
                    font-size 12px
    </style>