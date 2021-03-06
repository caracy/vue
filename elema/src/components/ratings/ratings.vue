<template>
    <div class="ratings" ref="ratings">
        <div class="ratings-content">
            <div class="overview">
                <div class="overview-left">
                    <h1 class="score">{{seller.score}}</h1>
                    <div class="title">综合评分</div>
                    <div class="rank">高于周边商家{{seller.rankRate}}%</div>
                </div>
                <div class="overview-right">
                    <div class="score-wrapper">
                        <span class="title">服务态度</span>
                        <star :size="36" :score="seller.serviceScore"></star>
                        <span class="score">{{seller.serviceScore}}</span>
                    </div>
                    <div class="score-wrapper">
                        <span class="title">商品评分</span>
                        <star  :size="36" :score="seller.foodScore"></star>
                        <span class="score">{{seller.foodScore}}</span>
                    </div>
                    <div class="delivery-wrapper">
                        <span class="title">送达时间</span>
                        <span class="delivery">{{seller.deliveryTime}}分钟</span>
                    </div>
                </div>
            </div>
            <split></split>
            <ratingselect @select="selectRatings" @toggle="toggleContent" :ratings="ratings" :selectType="selectType" :onlyContent="onlyContent" :desc="desc"></ratingselect>
            <div class="rating-wrapper">
                <ul>
                    <li v-for="(rating,index) in ratings" :key="index" v-show="needShow(rating.rateType,rating.text)"  class="rating-item border-1px">
                        <div class="avatar">
                            <img :src="rating.avatar" width="28" height="28">
                        </div>
                        <div class="content">
                            <h1 class="name">{{rating.username}}</h1>
                            <div class="star-wrapper">
                                <star :size="24" :score="rating.score"></star>
                                <span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}分钟送达</span>
                            </div>
                            <p class="text">{{rating.text}}</p>
                            <div class="recommend" v-show="rating.recommend && rating.recommend.length">
                                <span class="icon-thumb_up"></span>
                                <span class="item" v-for="(item,index) in rating.recommend" :key="index">{{item}}</span>
                            </div>
                            <div class="time">{{rating.rateTime | formaDate}}</div>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>
<script>
import BScroll from 'better-scroll'
import star from '../star/star'
import split from '../split/split'
import {formaDate} from '../../common/js/date'
import ratingselect from '../ratingselect/ratingselect'
import shopcar from '../shopcar/shopcar'

    const POSITIVE = 0;
    const NEGATIVE = 1;
    const ALL = 2;
export default{
    name:"ratings",
    data(){
        return{
            ratings:[],
            selectType:ALL,
            onlyContent:false,
            desc:{
                all:"全部",
                positive:"满意",
                negative:"不满意"
            }
        }
    },
    props:{
        seller:{
            type:Object
        },
    },

    created(){
        this.$axios.get('/api/ratings')
                    .then(res => {
                        this.ratings = res.data.data;
                        this.$nextTick( () => {
                            this.scroll = new BScroll(this.$refs.ratings,{
                                click:true
                            })
                        })         
                    })
                    .catch(err => {
                        console.log(err);
                    })          
    },
    methods:{
        selectRatings(type){
            this.selectType = type;
            this.$nextTick( () => {
                this.scroll.refresh()
            })
        },
        toggleContent(){
            this.onlyContent = !this.onlyContent;
            this.$nextTick( () => {
                this.scroll.refresh()
            })
        },
        needShow(type,text){
            if(this.onlyContent && !text){
                return false
            }
            if(this.selectType === ALL){
                return true
            }else{
                return type === this.selectType
            }
        }
    },
    filters: {
            formaDate(time){
                let date = new Date(time);
                return formaDate(date,'yyyy-MM-dd hh:mm')
            }
        },
    components:{
        star,
        split,
        ratingselect
    },
    
}
</script>
<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/mixin"
    .ratings
        position:absolute
        top 174px
        bottom 0
        left  0
        width 100%
        overflow hidden
        .ratings-content
            .overview
                padding 18px 0
                display flex
                .overview-left
                    flex 0 0 137.5px
                    padding 6px 0
                    width 137.5px
                    text-align center
                    border-right 1px solid rgba(7,17,27,0.1)
                    .score
                        line-height 28px
                        color rgb(255,153,0)
                        font-size 24px
                    .title
                        margin-top 6px
                        line-height 12px
                        font-size 12px
                        color #000
                    .rank
                        margin-top 8px
                        line-height 10px
                        font-size 10px
                        color rgb(7,17,27)
                .overview-right
                    flex 1
                    padding 6px 0 6px 24px
                    .score-wrapper
                        margin-bottom 8px
                        font-size 0
                        .title
                            display inline-block
                            vertical-align top
                            line-height 18px
                            font-size 12px
                            color rgb(7,17,27)
                        .star
                            display inline-block
                            vertical-align top
                            margin 0 12px
                        .score
                            display inline-block
                            vertical-align top
                            font-size 12px
                            line-height 18px
                            color rgb(255,153,0)
                    .delivery-wrapper
                        font-size 0
                        .title,.delivery
                            display inline-block
                            vertical-align top
                            line-height 18px
                            font-size 12px
                            color rgb(7,17,27)
                        .delivery
                            margin-left 12px
                            color rgb(147,153,159)
            .ratingselect
                margin-top 18px
            .rating-wrapper
                padding 0 18px
                .rating-item
                    padding 18px 0
                    display flex
                    border-1px(rgba(7,17,27,0.1))
                    .avatar
                        flex 0 0 28px
                        width 28px
                        margin-right 12px
                        img 
                            border-radius 50%
                    .content
                        flex 1
                        position relative
                        .name
                            line-height 12px
                            font-size 10px
                            color rgb(7,17,27)
                            margin-bottom 4px
                        .star-wrapper
                            font-size 0
                            .star
                                display inline-block
                                vertical-align top
                            .delivery
                                margin-left 6px
                                display inline-block
                                vertical-align top
                                line-height 12px
                                color rgb(147,153,159)
                                font-size 10px
                                font-weight 200
                    .text
                        line-height 18px
                        color rgb(7,17,27)
                        font-size 12px
                        margin-bottom 8px
                    .recommend
                        line-height: 16px
                        font-size: 0
                        .icon-thumb_up, .item
                            display: inline-block
                            margin: 0 8px 4px 0
                            font-size: 9px
                        .icon-thumb_up
                            color: rgb(0, 160, 220)
                        .item
                            padding: 0 6px
                            border: 1px solid rgba(7, 17, 27, 0.1)
                            border-radius: 1px
                            color: rgb(147, 153, 159)
                            background: #fff
                    .time
                        position absolute
                        right 18px
                        top 0
                        line-height 12px
                        font-size 10px        
                        font-weight 200
                        color rgb(147,153,159)
                    
</style>

