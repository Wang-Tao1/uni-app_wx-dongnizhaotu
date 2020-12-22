<template>
  <view class="video_play">
      <image :src="videoObj.img"></image>
     <!-- 工具栏 开始 -->
    <view class="video_tool">
        <view :class="['iconfont',muted?'iconjingyin':'iconshengyin']" @click="handleMuted"></view>
        <view class="iconfont iconzhuanfa">
            <button open-type="share"></button>
        </view>
    </view>
    <!-- 工具栏 结束 -->
    <!-- 视频 开始 -->
    <view class="video_wrap">
        <video :src="videoObj.video" object-fit="fill" :muted="muted"></video>
    </view>
    <!-- 视频 结束 -->
    <!-- 下载 开始 -->
    <view class="download">
      <view class="download_btn" @click="handleDownload">下载高清</view>
    </view>
    <!-- 下载 结束 -->
 </view>
</template>
 
<script>
export default {
 name: '',
 data () {
 return {
     videoObj: {},
     muted:false
 }
 },
 onLoad(){
     this.videoObj = getApp().globalData.video
 },
 methods:{
    handleMuted(){
        this.muted = !this.muted
    },
    async handleDownload(){
        await uni.showLoading({
            title:'下载中'
        })
        const { tempFilePath } = (await uni.downloadFile({
            url:this.videoObj.video
        }))[1]
        await uni.saveVideoToPhotosAlbum({
            filePath:tempFilePath
        })
        uni.hideLoading()
        await uni.showToast({title:"下载成功"})
    }
 }
}
</script>
 
<style scoped lang="scss">
.video_play {
    position: relative;
  image {
      position: absolute;
      z-index: -1;
      width: 100%;
      height: 100vh;
      filter: blur(20px);
  }

.video_tool {
    display: flex;
    flex-wrap: wrap;
    justify-content: flex-end;
    height: 80rpx;
    .iconfont {
      width: 80rpx;
      color: #fff;
      font-size: 50rpx;
      display: flex;
      justify-content: center;
      align-items: center;
      border-radius: 50%;
      background-color: rgba(0, 0, 0, 0.2);
      margin: 10rpx 20rpx;
    }
    .iconzhuanfa{
        position: relative;
        button{
            position: absolute;
            width: 100%;
            height: 100%;
            opacity: 0;
        }
    }
  }

.video_wrap {
    display: flex;
    justify-content: center;
    video {
        width: 360rpx;
        height: 600rpx;
    }
  }

.download {
    display: flex;
    justify-content: center;
    margin-top: 30rpx;
    .download_btn {
        width: 360rpx;
        height: 80rpx;
        border-radius: 40rpx;
        display: flex;
        justify-content: center;
        align-items: center;
        color: #fff;
        border: 1rpx solid #fff;
        background-color: rgba(0, 0, 0, 0.6);
    }
  }
}
</style>