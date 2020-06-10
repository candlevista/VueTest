<template>
  <div class="cartcontrol">
    <div class="cart-decrease">-</div>
    <div class="cart-count">{{food.count}}</div>
    <div class="cart-add">+</div>
  </div>
</template>

<script>
export default {
  props: {
    food: {
      type: Object
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
  .cart-count
    display inline-block
    width 12px
    padding-top 6px
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
