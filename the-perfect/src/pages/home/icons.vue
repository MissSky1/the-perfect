<template>
  <swiper :options="swiperOption" class="icons">
    <swiper-slide class="icons-page" v-for="(page, index) of pages" :key="index">
      <div class="icons-list">
        <div v-for="item of page" :key="item.id" class="icons-item">
          <img class="icons-img" :src="item.imgUrl">
          <p class="icons-title">{{item.title}}</p>
        </div>
      </div>
    </swiper-slide>
    <div class="swiper-pagination"  slot="pagination"></div>
  </swiper>
</template>

<script>
export default {
  name: 'index-icons',
  props: {
    icons: Array
  },
  data () {
    return {
      swiperOption: {
        loop: true,
        pagination: '.swiper-pagination'
      }
    }
  },
  computed: {
    pages () {
      const pages = []
      this.icons.forEach((value, index) => {
        const page = Math.floor(index / 8)
        if (!pages[page]) {
          pages[page] = []
        }
        pages[page].push(value)
      })
      return pages
    }
  }
}
</script>

<style lang="stylus" scoped>
  @import '../../assets/common/varibles.styl'
  @import '../../assets/common/mixins.styl'
  .icons >>> .swiper-pagination-bullet
    background: $bgColor
  .icons
    overflow: hidden      // 让元素撑开高度，刷新时不再抖动
    height: 0
    padding-bottom: 3.4rem
    width: 100%
    .icons-list
      display: flex
      flex-wrap: wrap
      .icons-item
        padding-top: .3rem
        width: 25%
        text-align: center
        .icons-img
          width: .66rem
          height: .66rem
        .icons-title
          margin-top: .16rem
          font-size: .28rem
          color: #333
          ellipsis()
</style>
