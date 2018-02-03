<template>
  <div class="main">
    <index-header></index-header>
    <index-slider :sliders="sliders"></index-slider>
    <index-icons :icons="icons"></index-icons>
    <index-scroller class="scroller" :sights="sights"></index-scroller>
    <download v-show="showDownload">
      下载条
    </download>
  </div>
</template>

<script>
import IndexHeader from './header'
import IndexSlider from './slider'
import IndexIcons from './icons'
import IndexScroller from './scroller'
import Download from './download'
import axios from 'axios'
import { mapState, mapMutations } from 'vuex'
export default {
  name: 'index',
  data () {
    return {
      sliders: [],
      icons: [],
      sights: []
    }
  },
  components: {
    IndexHeader,
    IndexSlider,
    IndexIcons,
    IndexScroller,
    Download
  },
  computed: {
    ...mapState(['city', 'showDownload'])
  },
  watch: {
    city () {
      this.getIndexData()
    }
  },
  methods: {
    ...mapMutations(['changeCity']),
    getIndexData () {
      axios.get('/api/index.json?city=' + this.city)
        .then(this.handleGetDataSucc.bind(this))
        .catch(this.handleGetDataError.bind(this))
    },
    handleGetDataSucc (res) {
      res = res ? res.data : null
      if (res && res.ret && res.data) {
        // res.data.city && (this.changeCity(res.data.city))
        res.data.slider && (this.sliders = res.data.slider)
        res.data.icons && (this.icons = res.data.icons)
        res.data.sights && (this.sights = res.data.sights)
      } else {
        this.handleGetDataError()
      }
    },
    handleGetDataError () {
      console.log('err')
    }
  },
  mounted () {
    this.getIndexData()
  }
}
</script>

<style lang="stylus" scoped>
  @import '../../assets/common/mixins.styl'
  .main
    hello(red)
    display: flex
    flex-direction: column
    position: absolute
    left: 0
    right: 0
    top: 0
    bottom: 0
    .scroller
      flex: 1
      overflow: hidden
</style>
