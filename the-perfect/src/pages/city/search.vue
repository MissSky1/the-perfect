<template>
  <div>
    <div class="search" ref="search">
      <input class="search-input" type="text" name="" placeholder="输入城市名或拼音" @input="handleInputChange"/>
    </div>
    <div class="search-list" v-show="showList" ref="list">
      <ul>
        <li class="search-item border-bottom" v-for="(item, index) of filterResult" :key="index">{{item.name}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
export default {
  name: 'city-search',
  props: {
    list: Array
  },
  data () {
    return {
      showList: false,
      filterResult: []
    }
  },
  computed: {
    result () {
      const result = []
      this.list.forEach((value, index) => {
        value[1].forEach((value, index) => {
          result.push({
            name: value.cityarea,
            spell: value.itemName
          })
        })
      })
      return result
    }
  },
  watch: {
    filterResult () {
      this.$nextTick(() => {
        this.scroll && this.scroll.refresh()
      })
    }
  },
  methods: {
    handleInputChange (e) {
      const inputValue = e.target.value.toLowerCase()
      if (inputValue) {
        this.showList = true
        this.filterResult = this.result.filter((value) => {
          if (value.spell.indexOf(inputValue) > -1 || value.name.indexOf(inputValue) > -1) {
            return true
          }
        })
      } else {
        this.showList = false
      }
    }
  },
  mounted () {
    this.scroll = new BScroll(this.$refs.list, {
      probeType: 3
    })
  }
}
</script>

<style lang="stylus" scoped>
  @import '../../assets/common/varibles.styl'
  .search
    position: relative
    z-index: 2
    box-sizing: border-box
    padding: 0 .2rem
    height: .74rem
    background: $bgColor
    .search-input
      box-sizing: border-box
      padding: 0 .2rem
      margin-top: .06rem
      width: 100%
      line-height: .62rem
      border-radius: .06rem
      font-size: .24rem
      color: $fontNormalColor
      text-align: center
  .search-list
    overflow: hidden
    z-index: 2
    position: absolute
    top: 1.62rem
    left: 0
    right: 0
    bottom: 0
    background: #f5f5f5
    .search-item
      padding-left: .3rem
      line-height: .62rem
      background: #fff
      color: #333
</style>
