<template>
	<div class="food" v-show="showFlag">
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
	</div>
</template>

<script>
import BScroll from 'better-scroll'
import cartcontrol from 'components/cartcontrol/cartcontrol'
import Vue from 'vue'

export default {
	props: {
		food: {
			type: Object
		}
	},
	data() {
		return {
			showFlag: false
		}
	},
	methods: {
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
		}
	},
	components: {
		cartcontrol
	}
}
</script>

<style lang="stylus">
.food
	position fixed
	top 0
	left 0
	bottom 48px
	z-index 30
	width 100%
	background #fff
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
			right 12px
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
</style>
