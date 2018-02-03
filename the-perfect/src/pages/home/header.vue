<template>
  <div class="header iconfont">
    <div class="back">&#xe618;</div>
    <div class="search">
      <div class="search-con">
        <span class="search-icon">&#xe7d4;</span>
        请输入城市景点
      </div>
    </div>
    <router-link to="/city">
      <div class="city">{{city}}</div>
    </router-link>
  </div>
</template>

<script>
import { mapState } from 'vuex'
export default {
  name: 'index-header',
  computed: {
    ...mapState(['city'])
  }
}
</script>

<style lang="stylus" scoped>
  @import '../../assets/common/varibles.styl'
  .header
    display: flex
    height: .88rem
    background: $bgColor
    color: #fff
    .back
      width: .8rem
      line-height: .8rem
      text-align: center
      font-size: .44rem
      font-weight: bold
    .search
      flex: 1
      .search-con
        margin-top: .14rem
        width: 100%
        height: .6rem
        line-height: .6rem
        background: #fff
        color: #ccc
        border-radius: .1rem
        text-indent: .2rem
        .search-icon
          position: relative
          top: .01rem
    .city
      overflow: hidden
      padding: 0 .46rem 0 .22rem
      max-width: 1.6rem
      line-height: .88rem
      font-size: .28rem
      color: #fff
      wrap-space: nowrap
      text-overflow: ellipsis
      &:before
        position: absolute
        right: .15rem
        top: .38rem
        content: "\0020"
        width: 0
        height: 0
        border-left: .12rem solid transparent
        border-right: .12rem solid transparent
        border-top: .12rem solid #fff
</style>
