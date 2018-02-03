<template>
  <div class="sidebar">
    <div class="sidebar-list" ref="alpList">
      <div class="sidebar-item" v-for="item of alphabet" :key="item" @touchstart="handleTouchStart(item)" @touchmove="handleTouchMove">{{item}}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'city-sidebar',
  props: {
    list: Array
  },
  computed: {
    alphabet () {
      const arr = []
      this.list.forEach((value, index) => {
        arr.push((value[0]))
      })
      return arr
    },
    areaTop () {
      return this.$refs.alpList.offsetTop - this.$refs.alpList.offsetHeight / 2 + 81
    }
  },
  methods: {
    handleTouchStart (item) {
      this.$emit('changeLetter', item)
    },
    handleTouchMove (e) {
      let index = Math.floor((e.touches[0].clientY - this.areaTop) / 20)
      index = index < 0 ? 0 : index
      index = (index >= this.alphabet.length) ? (this.alphabet.length - 1) : index
      this.$emit('changeLetter', this.alphabet[index])
    }
  }
}
</script>

<style lang="stylus" scoped>
  @import '../../assets/common/varibles.styl'
  .sidebar
    position: absolute
    right: 0
    top: 1.62rem
    bottom: 0
    width: .4rem
    .sidebar-list
      width: 100%
      position: absolute
      top: 50%
      transform: translateY(-50%)
      .sidebar-item
        width: 100%
        line-height: .36rem
        text-align: center
        color: #666
</style>
