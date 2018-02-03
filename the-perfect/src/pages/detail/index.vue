<template>
  <div>
    <detail-banner :bannerImg="bannerImg" :imgList="imgList"></detail-banner>
    <detail-header></detail-header>
    <detail-list :list="ticketList"></detail-list>
    <download v-show="showDownload">下载条</download>
  </div>
</template>

<script>
import DetailBanner from './banner'
import Download from 'components/common/download/'
import { mapState } from 'vuex'
import DetailList from './list'
import DetailHeader from './header'
import axios from 'axios'
export default {
  name: 'detail',
  props: {
    sightId: [Number, String]
  },
  data () {
    return {
      bannerImg: '',
      imgList: [],
      download: false,
      ticketList: []
    }
  },
  computed: {
    ...mapState(['showDownload'])
  },
  components: {
    DetailBanner,
    Download,
    DetailList,
    DetailHeader
  },
  created () {
    this.getDataListInfo()
  },
  watch: {
    'sightId' () {
      if (this.sightId) {
        this.getDataListInfo()
      }
    }
  },
  methods: {
    getDataListInfo () {
      axios.get('/api/detail.json', {
        params: {
          id: this.sightId
        }
      })
        .then(this.handleGetDetailSucc.bind(this))
        .catch(this.handleGetDetailErr.bind(this))
    },
    handleGetDetailSucc (res) {
      res && (res = res.data)
      if (res && res.ret && res.data) {
        this.bannerImg = res.data.bannerImg
        this.imgList = res.data.imgList
        this.ticketList = res.data.ticketList
      } else {
        this.handleGetDetailErr()
      }
    },
    handleGetDetailErr () {
      console.log('err')
    }
  }
}
</script>

<style lang="stylus" scoped></style>
