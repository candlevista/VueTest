<template>
  <div class="goods">
    <div class="menu-wrapper"></div>
    <div class="food-wrapper"></div>
  </div>
</template>

<script>
export default {

}
</script>

<style lang="stylus">
.goods
  display flex
  position absolute
  top 174px
  bottom 46px
  width 100%
  overflow hidden
  .menu-wrapper
    flex 0 0 80px
    width 80px
    background #f3f5f7
  .food-wrapper
    flex 1  
</style>
