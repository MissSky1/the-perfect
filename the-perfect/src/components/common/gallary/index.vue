<template>
  <div class="gallary" @click="handleGallaryClose">
    <swiper class="container" :options="swiperOption">
      <swiper-slide v-for="(item, index) of imgList" :key="index">
        <img class="gallary-img" :src="item" @click.stop>
      </swiper-slide>
      <div class="swiper-pagination"  slot="pagination"></div>
    </swiper>
  </div>
</template>

<script>
export default {
  name: 'gallary',
  props: {
    imgList: Array,
    default: []
  },
  data () {
    return {
      swiperOption: {
        loop: true,
        observer: true,
        observeParents: true,
        pagination: '.swiper-pagination',
        paginationType: 'fraction'
      }
    }
  },
  methods: {
    handleGallaryClose () {
      this.$emit('close')
    }
  }
}
</script>

<style lang="stylus" scoped>
  .container >>> .swiper-pagination-fraction
    color: #fff
    bottom: .6rem
  .gallary
    z-index: 999
    overflow: hidden
    position: fixed
    top: 0
    right: 0
    bottom: 0
    left: 0
    background: #000
    .container
      width: 100%
      height: 100%
      .gallary-img
        position: absolute
        top: 50%
        left: 0
        margin-top: -50%
        width: 100%
</style>
