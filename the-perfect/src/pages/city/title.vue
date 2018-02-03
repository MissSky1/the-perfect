<template>
  <div class="fixed-title border-bottom" v-show="show" ref="title">您的位置</div>
</template>

<script>
export default {
  name: 'fixed-title',
  data () {
    return {
      show: false
    }
  },
  methods: {
    getScroll (e) {
      if (e.y > 0) {
        this.show = false
      } else {
        this.show = true
      }
    },
    startMove (num) {
      this.$refs.title.style.top = (81 - num) + 'px'
    },
    changeLetter (letter) {
      this.$refs.title.innerText = letter
    }
  }
}
</script>

<style scoped lang="stylus">
  .fixed-title
    position: fixed
    top: 1.62rem
    left: 0
    right: 0
    line-height: .54rem
    background: #f5f5f5
    color: #616161
    padding-left: .3rem
    font-size: .26rem
</style>
