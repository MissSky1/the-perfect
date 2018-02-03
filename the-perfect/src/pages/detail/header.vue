<template>
  <div class="header" ref="header">
    景点详情
  </div>
</template>

<script>
export default {
  name: 'detail-header',
  activated () {
    this.bindScroll()
  },
  deactivated () {
    this.unbindScroll()
  },
  methods: {
    bindScroll () {
      this.scrollFn = this.handleScroll.bind(this)
      window.addEventListener('scroll', this.scrollFn)
    },
    handleScroll (e) {
      let opacity = (document.documentElement.scrollTop / 180)
      opacity = opacity > 1 ? 1 : opacity
      this.$refs.header.style.opacity = opacity
    },
    unbindScroll () {
      window.removeEventListener('scroll', this.scrollFn)
    }
  }
}
</script>

<style lang="stylus" scoped>
  @import '../../assets/common/varibles.styl'
  .header
    z-index: 1
    position: fixed
    top: 0
    width: 100%
    height: .8rem
    background: $bgColor
    opacity: 0
</style>
