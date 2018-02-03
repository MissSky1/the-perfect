<template>
  <div ref="wrapper" class="wrapper">
    <div class="list">
      <div class="area">
        <div class="title border-topbottom" ref="current">您的位置</div>
        <div class="content">
          <div class="button">
            <div class="button-text button-active" @click="handleCityClick(city)">{{city || "北京"}}</div>
          </div>
        </div>
      </div>
      <div class="area">
        <div class="title border-topbottom" ref="hotCity">热门城市</div>
        <div class="content">
          <div class="button" v-for="item of hotcity" :key="item.id" @click="handleCityClick(item.city)">
            <div class="button-text" :class="{'button-active': item.city === city}">{{item.city}}</div>
          </div>
        </div>
      </div>
      <div>
        <div class="area" v-for="(cityByAlphabet, index) of list" :key="index" :ref="cityByAlphabet[0]">
          <div class="title border-topbottom">{{cityByAlphabet[0]}}</div>
          <div class="contentByAlp" v-for="city of cityByAlphabet[1]" :key="city.id">
            <div class="city" @click="handleCityClick(city.cityarea)">{{city.cityarea}}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState, mapMutations } from 'vuex'
import BScroll from 'better-scroll'
export default {
  name: 'city-list',
  props: {
    list: Array,
    hotcity: Array
  },
  data () {
    return {
      showFixed: true,
      letter: ''
    }
  },
  computed: {
    areaPositions () {
      const arr = []
      const { current, hotCity } = this.$refs
      arr.push({
        top: current.offsetTop,
        text: current.innerText
      })
      arr.push({
        top: hotCity.offsetTop,
        text: hotCity.innerText
      })
      this.list.forEach((value, index) => {
        arr.push({
          top: this.$refs[value[0]][0].offsetTop,
          text: value[0]
        })
      })
      return arr
    },
    ...mapState(['city'])
  },
  mounted () {
    this.scroll = new BScroll(this.$refs.wrapper, {
      probeType: 3
    })
    this.scroll.on('scroll', this.handleScroll.bind(this))
  },
  methods: {
    handleCityClick (city) {
      this.changeCity(city)
      this.$nextTick(() => {
        this.$router.push('/')
      })
    },
    scrollToIndex (item) {
      this.scroll.scrollToElement(this.$refs[item][0])
    },
    handleScroll (e) {
      const y = -e.y
      let flag = false
      let diff = 0
      let letter = this.letter
      for (var i = 0; i < this.areaPositions.length; i++) {
        const position = this.areaPositions[i]
        if (y > position.top - 27 && y < position.top) {
          diff = y - position.top + 27
          flag = true
          this.$emit('fixChange', diff)
          break
        }
        if (y > position.top) {
          letter = position.text
        }
      }
      this.letter = letter
      this.$emit('changeLetter', this.letter)
      this.$emit('changeScroll', e)
      !flag && this.$emit('fixChange')
    },
    ...mapMutations(['changeCity'])
  },
  activated () {
    this.$nextTick(() => {
      this.scroll && this.scroll.refresh()
    })
  }
}
</script>

<style lang="stylus" scoped>
  @import '../../assets/common/varibles.styl'
  .wrapper
    .area
      .title
        padding-left: .3rem
        line-height: .54rem
        background: #eee
        color: #616161
        font-size: .26rem
        &:before, &:after
          border-color: #919191
      .content
        overflow: hidden
        padding: .1rem .3rem
        .button
          float: left
          width: 33.33%
          .button-text
            margin: .1rem
            line-height: .6rem
            border: .02rem solid #999
            border-radius: .06rem
            text-align: center
          .button-active
            color: $bgColor
            border-color: $bgColor
      .contentByAlp
        overflow: hidden
        .city
          box-sizing: border-box
          padding: 0 .3rem
          width: 100%
          line-height: .76rem
          border-bottom: .02rem solid #f5f5f5
          overflow: hidden
          white-space: nowrap
          text-overflow: ellipsis
</style>
