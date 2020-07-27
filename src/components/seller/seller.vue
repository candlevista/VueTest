<template>
  <div>
    <div class="overview">
      <p class="title"></p>
      <div class="star-wrappar">
        <star></star>
        <span></span>
      </div>
      <ul class="remark">
        <li>          
        </li>
      </ul>
    </div>
    <split></split>
    <div class="bulletin"></div>
    <split></split>
    <div class="pics"></div>
    <split></split>
    <div class="info"></div>
  </div>
</template>

<script>
import split from 'components/split/split'
  import star from 'components/star/star'

export default {
  components: {
    split,
    star
  }
}
</script>

<style>
</style>