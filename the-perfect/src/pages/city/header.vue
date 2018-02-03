<template>
  <div class="header">
    <div class="title">
      <div class="back iconfont" @click="handleBackClick">&#xe618;</div>
      城市选择
    </div>
  </div>
</template>

<script>
export default {
  name: 'city-header',
  methods: {
    handleBackClick () {
      this.$router.go(-1)
    }
  }
}
</script>

<style lang="stylus" scoped>
  @import '../../assets/common/varibles.styl'
  .header
    position: relative
    height: .88rem
    line-height: .88rem
    background: $bgColor
    text-align: center
    font-size: .34rem
    font-weight: bold
    color: #fff
    .back
      position: absolute
      left: 0
      top: 0
      width: .84rem
      line-height: .88rem
      font-size: .44rem
</style>
