<template>
  <div class="container">
    <!-- 轮播块 -->
    <div class="swiper-block">
      <swiper class="swiper" indicator-dots="true" autoplay="true" interval="5000" duration="1000">
        <block v-for="(swiperItem, index) in hotDynamicsList" :key="index">
          <swiper-item class="vetically">
            <hot-topic :hotTopicData="swiperItem"></hot-topic>
          </swiper-item>
        </block>
      </swiper>
    </div>
    <!-- 搜索块 -->
    <!-- <div class="search-block"></div> -->
    <!-- 动态列表块 -->
    <div class="dynamics-block">
      <dynamics v-for="(dynamics,index) in dynamicsList" :key="index" :dynamicsData="dynamics"></dynamics>
    </div>
  </div>
</template>

<script>
  import dynamics from '@/components/dynamics'
  import hotTopic from '@/components/hotTopic'
  import {GetData} from '@/utils/fetchData'
  export default {
    data () {
      return {
        hotDynamicsList: [],
        dynamicsList: []
      }
    },
    components: {
      dynamics,
      hotTopic
    },
    methods: {
      getData: function (callBack) {
        // 调用热门话题接口
        GetData('hotDynamics').then((res) => {
          this.hotDynamicsList = res.data
          if (callBack) callBack()
        })
        // 调用动态列表接口
        GetData('dynamics').then((res) => {
          this.dynamicsList = res.data
          if (callBack) callBack()
        })
      }
    },
    created () {
      if (wx.getStorageSync('userId')) {
        // 调用登陆接口
        GetData('user', {user_id: wx.getStorageSync('userId')}).then((res) => {
          wx.setStorageSync('userId', res.data[0].user_id)
          // 将用户信息保存到store
          this.$store.dispatch('saveUserInfo', res.data[0])
        })
      }
      this.getData()
    },
    mounted () {
    },
    onLoad () {
    },
    onShow () {
    },
    onUnload () {
    },
    onHide () {
    },
    async onPullDownRefresh () {
    // to doing..
      this.getData(() => {
        wx.stopPullDownRefresh()
      })
    // 停止下拉刷新
    },

    // 上拉加载，拉到底部触发
    onReachBottom () {

    }
  }
</script>

<style scoped>
  .container {
    background: #f2f2f2;
  }

  .swiper-block {
    width: 100%;
  }

  .dynamics-block {
    width: 100%;
  }
</style>
