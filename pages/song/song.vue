<template>
  <view>正在播放。。。</view>
</template>

<script>
export default {
  data() {
    return {}
  },
  methods: {
    getSongId(id) {
      return new Promise((resolve, reject) => {
        uni.request({
          url: `http://localhost:3000/song/url?id=${id}`,
          data: {
            id: id
          },
          success: ({ data }) => {
            if (data.code === 200) {
              const url = data.data[0].url
              resolve(url)
            } else {
              reject(new Error('获取播放地址错误'))
            }
          }
        })
      })
    },
    play(url) {
      const app = getApp()
      app.songCtx.stop()
      app.songCtx.src = url
      app.songCtx.play()
    }
  },
  async onLoad({ id }) {
    const url = await this.getSongId(id)
    this.play(url)
  }
}
</script>

<style></style>
