<template>
  <div>
    <div class="type-wrappar">
      <span
        @click="selectTypeAction(2, $event)"
        class="block positive"
        :class="{active:selectType == 2}"
      >
        {{desc.all}}
        <span class="count">{{ratings.length}}</span>
      </span>
      <span
        @click="selectTypeAction(1, $event)"
        class="block positive"
        :class="{active:selectType == 1}"
      >
        {{desc.positive}}
        <span class="count">{{positives.length}}</span>
      </span>
      <span
        @click="selectTypeAction(0, $event)"
        class="block negative"
        :class="{active:selectType == 0}"
      >
        {{desc.negative}}
        <span class="count">{{negatives.length}}</span>
      </span>
    </div>
    <div class="switch">
      <div class="check" :class="{active:onlyContent == true}" @click="onlyContentAction"></div>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>

<script>
const POSITIVE = 0
const NEGATIVE = 1
const ALL = 2

export default {
  props: {
    ratings: {
      type: Array,
      default() {
        return []
      }
    },
    selectType: {
      type: Number,
      default: ALL
    },
    onlyContent: {
      type: Boolean,
      default: false
    },
    desc: {
      type: Object,
      default() {
        return {
          all: '全部',
          positive: '满意',
          negative: '不满意'
        }
      }
    }
  },
  computed: {
    positives() {
      return this.ratings.filter(rating => {
        return rating.rateType === POSITIVE
      })
    },
    negatives() {
      return this.ratings.filter(rating => {
        return rating.rateType === NEGATIVE
      })
    }
  },
  methods: {
    selectTypeAction(type, event) {
      if (!event._constructed) {
        return
      }
      this.$emit('selectTypeAction', type)
    },
    onlyContentAction(event) {
      if (!event._constructed) {
        return
      }
      this.$emit('onlyContentAction')
    }
  }
}
</script>

<style lang="stylus">
@import '../../common/stylus/mixin.styl'

.type-wrappar
  padding 18px 0
  border-1px(rgba(7, 17, 27, 0.1))
  .block
    display inline-block
    padding 8px 12px
    margin-right 8px
    line-height 16px
    border-radius 1px
    font-size 12px
    color rgb(77, 85, 93)
    &.active
      color #fff
    &.positive
      background rgba(0, 160, 220, 0.2)
      &.active
        background rgba(0, 160, 220, 1)
    &.negative
      background rgba(77, 85, 93, 0.2)
      &.active
        background rgb(77, 85, 93)
.switch
  padding-top 18px
  .check
    display inline-block
    margin-right 4px
    width 24px
    height 24px
    border-radius 12px
    background-color rgb(147, 153, 159)
    &.active
      background-color rgba(0, 160, 220, 1)
  .text
    vertical-align top
    font-size 12px
    line-height 24px
    color rgb(147, 153, 159)
</style>>
