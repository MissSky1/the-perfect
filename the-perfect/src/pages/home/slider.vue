<template>
  <swiper :options="swiperOption" class="index-banner">
    <swiper-slide v-for="item of sliders" :key="item.id">
      <img class="banner-img" :src="item.imgUrl">
    </swiper-slide>
    <div class="swiper-pagination"  slot="pagination"></div>
  </swiper>
</template>

<script>
export default {
  name: 'index-slider',
  props: {
    sliders: Array
  },
  data () {
    return {
      swiperOption: {
        loop: true,
        pagination: '.swiper-pagination'
      }
    }
  }
}
</script>

<style lang="stylus" scoped>
  @import '../../assets/common/varibles.styl'
  .index-banner
    width: 100%
    overflow: hidden
    padding-bottom: 31.25%
    height: 0
    .banner-img
      width: 100%
</style>

<style lang="stylus">
  .index-banner
    .swiper-pagination-bullet
      background: #fff
</style>
