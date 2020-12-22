<template>
  <view>
    <!-- 专辑背景 开始 -->
    <view class="album_bg">
      <image
        :src="album.cover"
        mode="widthFix"
      ></image>
      <view class="album_info">
        <view class="album_name">{{album.name}}</view>
        <view class="album_btn">关注专辑</view>
      </view>
    </view>
    <!-- 专辑背景 结束 -->
    <!-- 专辑作者 开始 -->
    <view class="album_author">
      <view class="album_author_info">
        <image
          mode="widthFix"
          :src="album.user.avatar"
        ></image>
        <view class="author_name">{{album.user.name}}</view>
      </view>
      <view class="album_author_desc">
        <text>{{album.desc}}</text>
      </view>
    </view>
    <!-- 专辑作者 结束 -->
    <!-- 列表 开始 -->
    <view class="album_list">
      <view
        class="album_item"
        v-for="(item,index) in wallpaper"
        :key="item.id"
      >
       <GoDetail :list="wallpaper" :index="index">
        <image 
          mode="aspectFill"         
          :src="item.thumb+item.rule.replace('$<Height>',360)"
        ></image>
        </GoDetail>
      </view>
    </view>
    <!-- 列表 结束 -->
    </view>
</template>
 
<script>
import GoDetail from "@/components/GoDetail";
export default {
  components:{GoDetail},
 name: '',
 data () {
 return {
     params: {
        limit: 30,
        order: "new",
        skip: 0,
        first: 1
      },
      id: -1,
      album: {},
      wallpaper: [],
      hasMore: true
 }
 },
 onLoad(options){
     this.id = options.id; 
     this.getList()
 },
 onReachBottom(){
    if(this.hasMore){
        this.params.first = 0
        this.params.skip += this.params.limit;
        this.getList();
    }else{
        uni.showToast({
        title: "没有更多数据了",
        icon: "none"
      }); 
    }
 },
 methods:{
     getList(){
         this.request({
             url:`http://157.122.54.189:9088/image/v1/wallpaper/album/${this.id}/wallpaper`,
             data:this.params
         }).then(result=>{
             if (Object.keys(this.album).length === 0) {
                this.album = result.res.album;
             }
             if (result.res.wallpaper.length === 0) {
                 this.hasMore = false
                 uni.showToast({
                   title: "没有更多数据了",
                   icon: "none"  
                 })
                 return
             }
             this.wallpaper = [...this.wallpaper, ...result.res.wallpaper];  
             console.log(result);
         })
     }
 }
}
</script>
 
<style scoped lang='scss'>
 .album_bg {
     position: relative;
  .album_info {
    position: absolute;
    left: 0;
    bottom: 0;
    width: 100%;
    display: flex;
    height: 80rpx;
    justify-content: space-between;
    color: #fff;
    padding: 0 15rpx;
    align-items: center;
    .album_name {
        font-size: 40rpx;
    }
    .album_btn {
      background-color: $color;
      width: 152rpx;
      height: 60rpx;
      border-radius: 10rpx;
      display: flex;
      justify-content: center;
      align-items: center;    
    }
  }
}
.album_author {
    padding: 10rpx;
  .album_author_info {
      display: flex;
      padding: 10rpx 0;
    image {
        width: 50rpx;
    }

    .author_name {
      color: #000;
      margin-left: 15rpx;
    }
  }

}
.album_list {
  display: flex;
  flex-wrap: wrap;
  .album_item {
      width: 33.33%;
      border: 3rpx solid #fff;
      image {
          height: 160rpx;
      }
  }
}
</style>