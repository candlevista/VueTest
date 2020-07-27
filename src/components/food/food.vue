<template>
  <transition name="move">
    <div class="food" v-show="showFlag" ref="food">
      <div class="food-content">
        <div class="image-header">
          <img :src="food.image" />
          <div class="back" @click="hide"></div>
        </div>
        <div class="content">
          <h1 class="title">{{food.name}}</h1>
          <div class="detil">
            <span class="sell-count">月售{{food.sellCount}}份</span>
            <span class="rating">好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class="now">￥{{food.price}}</span>
            <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
          </div>
          <div class="cartcontrol" v-show="food.count>0">
            <cartcontrol :food="food" @add="addFood"></cartcontrol>
          </div>
          <div class="buy" @click="addFirst" v-show="!food.count || food.count===0">加入购物车</div>
        </div>
        <split></split>
        <div class="info" v-show="food.info">
          <h1 class="title">商品介绍</h1>
          <p class="text">{{food.info}}</p>
        </div>
        <split></split>
        <div class="rating">
          <h1 class="title">商品评价</h1>
          <div>
            <ratingselect
              @selectTypeAction="selectTypeAction"
              @onlyContentAction="onlyContentAction"
              :selectType="selectType"
              :onlyContent="onlyContent"
              :desc="desc"
              :ratings="food.ratings"
            ></ratingselect>
          </div>
          <div class="rating-wrapper">
            <ul v-show="food.ratings && food.ratings.length">
              <li
                v-show="needShow(rating.rateType,rating.text)"
                class="rating-item"
                v-for="(rating, index) in food.ratings"
                :key="index"
              >
                <div class="user">
                  <span class="name">{{rating.username}}</span>
                  <img class="avatar" width="12" height="12" :src="rating.avatar" />
                </div>
                <div class="time">{{rating.rateTime | formatDate}}</div>
                <div class="content-wrappar">
                  <div class="thumbs" :class="{active:rating.rateType===0}"></div>
                  <p class="text">{{rating.text}}</p>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import BScroll from 'better-scroll'
import cartcontrol from 'components/cartcontrol/cartcontrol'
import split from 'components/split/split'
import ratingselect from 'components/ratingselect/ratingselect'
import { formatDate } from 'common/js/date'
import Vue from 'vue'

const ALL = 2

export default {
  props: {
    food: {
      type: Object
    }
  },
  data() {
    return {
      showFlag: false,
      onlyContent: false,
      selectType: ALL,
      desc: {
        all: '全部',
        positive: '推荐',
        negative: '吐槽'
      }
    }
  },
  filters: {
    formatDate(time) {
      let date = new Date(time)
      return formatDate(date, 'yyyy-MM-dd hh:mm')
    }
  },
  methods: {
    needShow(type, text) {
      if (this.onlyContent && !text) {
        return false
      }
      if (this.selectType === ALL) {
        return true
      } else {
        return type === this.selectType
      }
    },
    hide() {
      this.showFlag = false
    },
    show() {
      this.showFlag = true
      this.$nextTick(() => {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.food, {
            click: true
          })
        } else {
          this.scroll.refresh()
        }
      })
    },
    addFood() {
      this.$emit('add', event.target)
    },
    addFirst(event) {
      if (!event._constructed) {
        return
      }
      this.$emit('add', event.target)
      Vue.set(this.food, 'count', 1)
    },
    selectTypeAction(type) {
      this.selectType = type
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    },
    onlyContentAction() {
      this.onlyContent = !this.onlyContent
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    }
  },
  components: {
    cartcontrol,
    split,
    ratingselect
  }
}
</script>

<style lang="stylus">
@import '../../common/stylus/mixin.styl'

.food
  position fixed
  top 0
  left 0
  bottom 48px
  z-index 30
  width 100%
  background #fff
  transform translate3d(0, 0, 0)
  &.move-enter-active, &.move-leave-active
    transition all 0.3s linear
  &.move-enter, &.move-leave-to
    transform translate3d(100%, 0, 0)
  .image-header
    position relative
    width 100%
    padding-top 100%
    height 0
    img
      position absolute
      top 0
      left 0
      width 100%
      height 100%
    .back
      position absolute
      top 15px
      left 15px
      width 30px
      height 30px
      background-color red
  .content
    position relative
    padding 18px
    .title
      line-height 14px
      margin-bottom 8px
      font-size 14px
      font-weight 700
      color rgb(7, 17, 27)
    .detil
      margin-bottom 18px
      line-height 10px
      height 10px
      font-size 0
      .sell-count, .rating
        font-size 10px
        color rgb(147, 153, 159)
      .sell-count
        margin-right 12px
    .price
      font-weight 700
      line-height 24px
      .now
        margin-right 8px
        font-size 14px
        color rgb(240, 20, 20)
      .old
        text-decoration line-through
        font-size 10px
        color rgb(147, 153, 159)
    .cartcontrol
      position absolute
      right 12px
      bottom 12px
      width 80px
    .buy
      position absolute
      right 18px
      bottom 18px
      z-index 10
      height 24px
      line-height 24px
      padding 0 12px
      box-sizing border-box
      border-radius 12px
      font-size 10px
      color #fff
      background rgb(0, 160, 220)
      opacity 1
  .info
    padding 18px
    .title
      line-height 14px
      margin-bottom 6px
      font-size 14px
      color rgb(7, 17, 27)
    .text
      line-height 24px
      padding 0 8px
      font-size 12px
      color rgb(77, 85, 93)
  .rating
    padding 18px
    border-1px(rgba(7, 17, 27, 0.1))
    .title
      line-height 14px
      margin-bottom 6px
      font-size 14px
      color rgb(7, 17, 27)
    .rating-wrapper .rating-item
      position relative
      padding 16px 0
      border-1px(rgba(7, 17, 27, 0.1))
      .user
        position absolute
        right 0
        top 16px
        line-height 12px
        font-size 0
        .name
          display inline-block
          margin-right 6px
          vertical-align top
          font-size 10px
          color rgb(147, 153, 159)
        .avatar
          border-radius 50%
      .time
        margin-bottom 6px
        line-height 12px
        font-size 10px
        color rgb(147, 153, 159)
      .content-wrappar
        margin-top 6px
        font-size 0
        .thumbs
          display inline-block
          margin-right 6px
          width 16px
          height 16px
          background-color rgb(147, 153, 159)
          &.active
            background rgb(0, 160, 220)
        .text
          display inline-block
          vertical-align top
          font-size 12px
          line-height 16px
          color rgb(7, 17, 27)
</style>
