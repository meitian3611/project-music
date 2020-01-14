<template>
  <view class="page-commend">
    <view class="list">
      <view class="list-item" @click="handleItem(item.id)" v-for="(item, index) in musicList" :key="item.id">
        <view class="left">
          <view class="title">{{ item.name }}</view>
          <view class="info">{{ item.song.artists | formatArtists }} - {{ item.song.album.name }}</view>
        </view>
        <view class="iconfont iconbofang"></view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      musicList: []
    }
  },
  // 过滤器
  filters: {
    formatArtists(value) {
      const temp = value.map(item => item.name)
      return temp.join('/')
    }
  },
  methods: {
    getMusicList() {
      uni.request({
        url: 'http://localhost:3000/personalized/newsong',
        success: ({ data }) => {
          if (data.code === 200) {
            this.musicList = data.result
          } else {
            uni.showToast({
              title: '获取数据失败',
              icon: 'none'
            })
          }
        }
      })
    },
    handleItem(id) {
    uni.navigateTo({
      url:`../song/song?id=${id}`
    })
    }
  },
  created() {
    this.getMusicList()
  }
}
</script>

<style lang="scss">
.page-commend {
  height: 100%;
  .list {
    padding: 0 20rpx;
    &-item {
      padding: 20rpx;
      line-height: 1.6;
      border-bottom: 2rpx solid rgba(0, 0, 0, 0.1);
      display: flex;
      align-items: center;
      .left {
        flex: 1;
        .title {
          font-size: 36rpx;
          color: #333;
        }
        .info {
          font-size: 24rpx;
          color: #888;
        }
      }
      .iconfont {
        font-size: 44rpx;
        color: #aaa;
      }
    }
  }
}
</style>
