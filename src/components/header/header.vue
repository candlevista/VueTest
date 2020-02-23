<template>
  <div class="header">
    <div class="content-wrapper">
      <img width="64" height="64" class="avatar" :src="seller.avatar" />
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">{{seller.description}}/{{seller.deliveryTime}}分钟送达</div>
        <div v-if="seller.supports" class="support">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
        <div v-if="seller.supports" class="support-count" @click="showDescription">
          <span class="count">{{seller.supports.length}}个</span>
          <span class="support-arrow arrow"></span>
        </div>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDescription">
      <span class="bulletin-title"></span>
      <span class="bulletin-text">{{seller.bulletin}}</span>
      <span class="bulletin-arrow arrow"></span>
    </div>
    <div class="background">
      <img width="100%" height="100%" :src="seller.avatar" alt />
    </div>
    <transition name="fade">
      <div class="detail" v-show="detailShow">
        <div class="detail-wrapper">
          <h1 class="name">{{seller.name}}</h1>
          <div class="star-wrapper"></div>
          <div class="title">
            <div class="line"></div>
            <div class="text">优惠信息</div>
            <div class="line"></div>
          </div>
          <ul class="supports" v-if="seller.supports">
            <li class="supports-item" v-for="(item, index) in seller.supports" :key="index">
              <span class="icon" :class="classMap[seller.supports[index].type]"></span>
              <span class="text">{{seller.supports[index].description}}</span>
            </li>
          </ul>
          <div class="title">
            <div class="line"></div>
            <div class="text">商家信息</div>
            <div class="line"></div>
          </div>
          <div class="bulletin">
            <p class="content">{{seller.bulletin}}</p>
          </div>
        </div>
        <div class="detail-close" @click="hideDescription">
          <div class="close-button"></div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  props: {
    seller: {
      type: Object
    }
  },
  data() {
    return {
      detailShow: false
    }
  },
  created() {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
  },
  methods: {
    showDescription() {
      this.detailShow = true
    },
    hideDescription() {
      this.detailShow = false
    }
  }
}
</script>

<style lang="stylus">
@import '../../common/stylus/mixin'

.arrow
  filter invert(100%)
  background-image url('../../assets/arrow_right_gray.png')
  background-size contain
  background-repeat no-repeat
.header
  position relative
  overflow hidden
  color #fff
  background rgba(7, 17, 27, 0.5)
  font-size 0
  .content-wrapper
    position relative
    padding 24px 12px 18px 24px
    .avatar
      display inline-block
      border-radius 2px
    .content
      display inline-block
      vertical-align top
      margin-left 16px
      margin-top 2px
      .title
        margin 2px 0 8px 0
        .brand
          display inline-block
          width 30px
          height 18px
          bg-image('brand')
          background-size 30px 18px
          background-repeat no-repeat
        .name
          vertical-align top
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
          line-height 12px
          font-size 10px
      .support-count
        position absolute
        padding 0 8px
        right 12px
        bottom 14px
        height 24px
        border-radius 14px
        background rgba(0, 0, 0, 0.2)
        .count
          vertical-align top
          font-size 10px
          line-height 24px
        .support-arrow
          display inline-block
          filter invert(100%)
          width 15px
          height 24px
          margin-top 4px
  .bulletin-wrapper
    position relative
    height 28px
    padding 0 22px 0 12px
    background rgba(7, 17, 27, 0.2)
    overflow hidden
    text-overflow ellipsis
    // 不换行
    white-space nowrap
    .bulletin-title
      display inline-block
      margin-top 8px
      width 22px
      height 12px
      bg-image('bulletin')
      background-size 22px 12px
      background-repeat no-repeat
    .bulletin-text
      vertical-align top
      margin 0 4px
      font-size 10px
      font-weight 200
      color rgb(255, 255, 255)
      line-height 28px
    .bulletin-arrow
      position absolute
      right 12px
      top 7px
      width 15px
      height 15px
  .background
    position absolute
    top 0
    left 0
    width 100%
    height 100%
    z-index -1
    filter blur(10px)
  .detail
    position fixed
    display flex
    flex-direction column
    overflow auto
    z-index 100
    top 0
    left 0
    width 100%
    height 100%
    backdrop-filter blur(10px)
    background rgba(7, 17, 27, 0.8)
    &.fade-enter-active, &.fade-leave-active
      transition all 0.5s
    &.fade-enter, &.fade-leave-active
      opacity 0
      background rgba(7, 17, 27, 0)
    .detail-wrapper
      flex 1
      .name
        margin-top 64px
        line-height 16px
        text-align center
        font-size 16px
        font-weight 700
      .title
        display flex
        margin 28px 36px 0 36px
        .line
          flex 1
          margin-top 6px
          height 1px
          background rgba(255, 255, 255, 0.2)
        .text
          padding 0 12px
          font-weight 700
          font-size 14px
      .supports
        width 80%
        margin 24px auto 0 auto
        .supports-item
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
            line-height 16px
            font-size 12px
      .bulletin
        width 80%
        margin 0 auto
        .content
          padding 0 12px
          line-height 24px
          font-size 12px
    .detail-close
      width 100%
      height 64px
      .close-button
        margin 0 auto 32px auto
        width 32px
        height 32px
        background rgb(255, 100, 100)
</style>
