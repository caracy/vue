<template>
    <div class="cartcontrol">
        <transition name="move">
            <div class="cart-decrease" v-show="food.count>0" @click="decreaseCart"><i class="icon-remove_circle_outline"></i></div>
        </transition>
        <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
        <div class="cart-add" @click="addCart($event)">
            <i class="icon-add_circle"></i>
        </div>
    </div>
</template>
<script>
import Vue from 'vue'
    export default{
        name:'cartcontrol',
        props:{
            food:{
                type:Object
            }
        },
        created() {
            //console.log(this.food);   
        },
        methods:{
            addCart(event){
                if(!event._constructed){
                    return
                }
                if(!this.food.count){
                   Vue.set(this.food,'count',1)
                }else{
                    this.food.count++
                }
                this.$emit('cart-add',event.target)
            },

            decreaseCart(event){
                if(!event._constructed){
                    return
                }
                if(this.food.count>0){
                    this.food.count--;
                }
            }
        }
    }
</script>
<style lang="stylus">
    .cartcontrol
        font-size:0
        .move-enter-active,.move-leave-active
            transition all .5s linear
        .move-enter,.move-leave-to
            transform translate3d(36px,0,0) rotate(360deg)
            opacity 0    
        .cart-decrease,.cart-add
            display inline-block
            padding 6px
            font-size 24px
            line-height 24px
            color rgb(0,160,220)
        .cart-count
            display inline-block
            vertical-align top
            width 12px
            padding-top 6px
            line-height 24px
            text-align center 
            font-size 10px
            color rgb(147,153,159)

</style>

