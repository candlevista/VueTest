<template>
  <div class="goods">
    <div class="menu-wrapper">
      <ul>
        <li v-for="(item, index) in goods" :key="index" class="menu-item">
          <span class="text">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>
            {{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="food-wrapper"></div>
  </div>
</template>

<script>
import { getGoods } from 'api/index.js'

export default {
  props: {
    seller: {
      type: Object
    }
  },
  data() {
    return {
      goods: []
    }
  },
  created() {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']

    this.getGoods()
  },
  methods: {
    getGoods() {
      getGoods().then(data => {
        this.goods = data
      })
    }
  }
}
</script>

<style lang="stylus">
@import '../../common/stylus/mixin'

.goods
  display flex
  position absolute
  top 174px
  bottom 46px
  width 100%
  overflow hidden
  .menu-wrapper
    display flex
    flex 0 0 80px
    width 80px
    background #f3f5f7
    .menu-item
      display flex
      justify-content center
      align-items center
      height 54px
      width 64px
      margin 0 8px            
      border-1px(rgba(7, 17, 27, 0.1))
      .icon
        display inline-block
        vertical-align top
        width 12px
        height 12px
        background-size 12px 12px
        background-repeat no-repeat
        &.decrease
          bg-image('decrease_3')
        &.discount
          bg-image('discount_3')
        &.guarantee
          bg-image('guarantee_3')
        &.invoice
          bg-image('invoice_3')
        &.special
          bg-image('special_3')
      .text
        display table-cell
        width 56px
        vertical-align middle        
        font-size 12px
        line-height 14px        
  .food-wrapper
    flex 1
</style>
