<template>
  <div class="shopcart">
    <div class="content">
      <div class="content-left">
        <div class="logo-wrappar">
          <div class="logo" :class="{'highlight':totalCount>0}"></div>
          <div class="num" v-show="totalCount>0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{'highlight':totalPrice>0}">￥{{totalPrice}}</div>
        <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
      </div>
      <div class="content-right" :class="payClass">
        <div class="pay">{{payDesc}}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    selectFoods: {
      type: Array,
      default() {
        return [
          // {
          //   price: 10,
          //   count: 1
          // }
        ]
      }
    },
    // 配送费
    deliveryPrice: {
      type: Number,
      default: 0
    },
    // 起送价
    minPrice: {
      type: Number,
      default: 0
    }
  },
  computed: {
    totalCount() {
      let count = 0
      this.selectFoods.forEach(element => {
        count += element.count
      })
      return count
    },
    totalPrice() {
      let price = 0
      this.selectFoods.forEach(element => {
        price += element.count * element.price
      })
      return price
    },
    payDesc() {
      if (this.totalPrice === 0) {
        return `￥${this.minPrice}元起送`
      } else if (this.totalPrice < this.minPrice) {
        let diff = this.minPrice - this.totalPrice
        return `还差￥${diff}元起送`
      } else {
        return '去结算'
      }
    },
    payClass() {
      if (this.totalPrice < this.minPrice) {
        return 'not-enough'
      } else {
        return 'enough'
      }
    }
  }
}
</script>

<style lang="stylus">
.shopcart
  position fixed
  left 0
  bottom 0
  z-index 50
  width 100%
  height 48px
  .content
    display flex
    background #141d27
    color rgba(255, 255, 255, 0.4)
    height 48px
    .content-left
      flex 1
      .logo-wrappar
        display inline-block
        position relative
        box-sizing border-box
        vertical-align top
        width 56px
        height 56px
        top -10px
        margin 0 12px
        border-radius 50%
        background #141d27
        padding 6px
        .logo
          width 100%
          height 100%
          border-radius 50%
          background #2b343c
          &.highlight
            background rgb(0, 160, 220)
        .num
          position absolute
          top 0
          right 0
          width 24px
          height 16px
          line-height 16px
          text-align center
          border-radius 16px
          font-size 9px
          font-weight 700
          color #fff
          background rgb(240, 20, 20)
          box-shadow 0 4px 8px 0 rgba(0, 0, 0, 0.4)
      .price
        display inline-block
        vertical-align top
        margin-top 12px
        line-height 24px
        padding-right 12px
        font-size 16px
        font-weight 700
        box-sizing border-box
        border-right 1px solid rgba(255, 255, 255, 0.1)
        &.highlight
          color #fff
      .desc
        display inline-block
        vertical-align top
        margin 12px 0 0 12px
        line-height 24px
        font-size 10px
    .content-right
      flex 0 0 105px
      display flex
      justify-content center
      align-items center
      width 105px      
      &.not-enough
        background #2b333b
      &.enough
        background #00b43c
        color #fff
      .pay
        font-size 12px
        font-weight 700
</style>