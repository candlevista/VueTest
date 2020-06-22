<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decreaseCart">
        <div class="inner"></div>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add" @click.stop.prevent="addCart"></div>
  </div>
</template>

<script>
import Vue from 'vue'

export default {
  props: {
    food: {
      type: Object
    }
  },
  methods: {
    addCart(event) {
      if (!event._constructed) {
        return
      }
      if (!this.food.count) {
        Vue.set(this.food, 'count', 1)
      } else {
        this.food.count++
      }
      this.$emit('add', event.target)
    },
    decreaseCart(event) {
      if (!event._constructed) {
        return
      }
      if (this.food.count) {
        this.food.count--
      }
    }
  }
}
</script>

<style lang="stylus">
.cartcontrol
  height 24px
  line-height 24px
  .cart-decrease
    display inline-block
    width 20px
    height 20px
    line-height 20px
    font-size 20px
    background-color #ffffff
    color rgb(0, 160, 220)
    border-radius 50%
    text-align center
    border 2px solid rgb(0, 160, 220)
    box-sizing content-box
    opacity 1
    transform translate3d(0, 0, 0)    
    .inner      
      transform rotate(0)
    &.move-enter-active, &.move-leave-active
      transition all 0.4s linear
      .inner
        transition all 0.4s linear
    &.move-enter, &.move-leave-to
      opacity 0
      transform translate3d(24px, 0, 0)      
      .inner
        transform rotate(180deg)
  .cart-count
    display inline-block
    vertical-align top
    width 12px
    line-height 24px
    text-align center
    font-size 10px
    color rgb(147, 153, 159)
  .cart-add
    display inline-block
    width 24px
    height 24px
    color #fff
    background-color rgb(0, 160, 220)
    border-radius 50%
    text-align center
    line-height 24px
    font-size 24px
    box-sizing content-box
</style>
