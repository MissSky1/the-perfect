<template>
  <div ref="scroller">
    <div class="content">
      <transition class="loading-active" name="loading">
        <div v-show="isLoading" class="loading">正在加载...</div>
      </transition>
      <router-link class="item" v-for="item in list" :key="item.id" :to="'/detail/' + item.id" tag="div">
        <img class="item-img" v-lazy="item.imgUrl">
        <div class="item-content">
          <p class="item-title">{{item.title}}</p>
          <p class="item-desc">{{item.desc}}</p>
          <p class="item-price">
            <span class="price-tag">&yen;</span>
            {{item.price}}
            <span class="price-start">起</span></p>
        </div>
      </router-link>
    </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
import axios from 'axios'
import { mapState } from 'vuex'
export default {
  name: 'index-scroller',
  props: {
    sights: Array
  },
  data () {
    return {
      pageNum: 1,
      moreSights: [],
      isLoading: false,
      isFetching: false// 控制ajax的请求，防止连续刷新
    }
  },
  watch: {
    list () {
      this.$nextTick(() => {
        this.scroller.refresh()
      })
    }
  },
  computed: {
    list () {
      return this.moreSights.concat(this.sights)
    },
    ...mapState(['city'])
  },
  methods: {
    createScroller () {
      this.scroller = new BScroll(this.$refs.scroller, {
        probeType: 3
      })
    },
    bindEvents () {
      this.scroller.on('scroll', this.handleScroll.bind(this))
      this.scroller.on('scrollEnd', this.handleScrollEnd.bind(this))
    },
    handleScroll (e) {
      if (e.y > 60 && !this.isLoading) {
        this.getListInfo()
        this.isLoading = true
      }
    },
    handleScrollEnd (e) {
      this.isLoading = false
    },
    getListInfo () {
      if (this.isFetching) {
        this.isFetching = true
        axios.get('/api/sightList.json?city' + this.city + '&page=' + this.pageNum)
          .then(this.handleGetListSucc.bind(this))
          .catch(this.handleGetListError.bind(this))
      }
    },
    handleGetListSucc (res) {
      res && (res = res.data)
      if (res.data) {
        if (res.data.list) {
          this.moreSights = this.moreSights.concat(res.data.list)
          this.pageNum += 1
          this.isFetching = false
        } else {
          this.handleGetListError()
        }
      }
    },
    handleGetListError () {
      this.isFetching = false
      console.log('获取景点失败')
    }
  },
  mounted () {
    this.createScroller()
    this.bindEvents()
  }
}
</script>

<style lang="stylus" scoped>
  @import '../../assets/common/varibles.styl'
  .loading
    line-height: .8rem
    text-align: center
    color: $lightFontColor
  .loading-enter-active, .loading-leave-active
    transition: opacity .5s
  .loading-enter, .loading-leave-to
    opacity: 0
  .item
    display: flex
    padding: .24rem
    .item-img
      width: 1.4rem
      height: 1.4rem
      margin-right: .2rem
    .item-content
      flex: 1
      .item-title
        margin: .04rem 0 .1rem 0
        font-size: .3rem
        color: $fontColor
      .item-desc
        margin-bottom: .1rem
        font-sizze: .28rem
        color: $lightFontColor
      .item-price
        margin-top: .2rem
        font-size: .36rem
        color: $enlightFontColor
        .price-tag
          font-size: .24rem
        .price-start
          font-size: .24rem
          color: $lightFontColor
</style>
