<template>
  <div class="main">
    <city-header></city-header>
    <city-search :list="list"></city-search>
    <city-list class="list"
                :list="list"
                :hotcity="hotcity"
                ref="list"
                @changeScroll="handleScroll"
                @fixChange="handleFix"
                @changeLetter="handleChangeLetter">
    </city-list>
    <city-sidebar :list="list" @changeLetter="handleLetterChange"></city-sidebar>
    <fixed-title ref="title"></fixed-title>
  </div>
</template>

<script>
import CityHeader from './header'
import CitySearch from './search'
import CityList from './list'
import CitySidebar from './sidebar'
import FixedTitle from './title'
import axios from 'axios'
export default {
  name: 'city',
  data () {
    return {
      list: [],
      hotcity: []
    }
  },
  components: {
    CityHeader,
    CitySearch,
    CityList,
    CitySidebar,
    FixedTitle
  },
  methods: {
    getListInfo () {
      axios.get('/api/cityList.json')
        .then(this.handleGetListSucc.bind(this))
        .catch(this.handleGetListErr.bind(this))
    },
    handleGetListSucc (res) {
      res && (res = res.data)
      if (res && res.data && res.ret) {
        res.data.china && (this.list = res.data.china)
        res.data.hotcity && (this.hotcity = res.data.hotcity)
      } else {
        this.handleGetListErr()
      }
    },
    handleGetListErr () {
      console.log('error')
    },
    handleLetterChange (item) {
      this.$refs.list.scrollToIndex(item)
    },
    handleScroll (e) {
      this.$refs.title.getScroll(e)
    },
    handleFix (num) {
      this.$refs.title.startMove(num || 0)
    },
    handleChangeLetter (letter) {
      this.$refs.title.changeLetter(letter)
    }
  },
  created () {
    this.getListInfo()
  }
}
</script>

<style lang="stylus" scoped>
  .main
    display: flex
    flex-direction: column
    position: absolute
    left: 0
    top: 0
    right: 0
    bottom: 0
    .list
      overflow: hidden
      flex: 1
</style>
