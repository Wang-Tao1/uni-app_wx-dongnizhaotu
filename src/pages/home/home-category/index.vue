<template>
 <view class="home_category">
     <navigator
     v-for="item in category" :key="item.id"
     class="cate_item"
     :url="`/pages/imgCategory/index?id=${item.id}`"
     >
    <image mode="aspectFill" :src="item.cover"></image>
    <view class="cate_name">{{item.name}}</view>
     </navigator>
 </view>
</template>
 
<script>
export default {
 name: '',
 data () {
 return {
   category: []   
 }
 },
 mounted() {
     uni.setNavigationBarTitle({title:'分类'})
     this.getList()
 },
 methods:{
     getList(){
         this.request({
             url:'http://157.122.54.189:9088/image/v1/vertical/category'
         }).then(result=>{
             this.category = result.res.category;
         })
     }
 }
}
</script>
 
<style scoped lang="scss">
 .home_category {
     display: flex;
     flex-wrap: wrap;
   .cate_item {
       position : relative;
       width : 33.33%;
       border: 5rpx solid #fff;
    image {
        height: 240rpx;
    }
    .cate_name {
        font-size: 40rpx;
        position: absolute;
        width: 100%;
        height: 50rpx;
        left: 0;
        bottom: 0;
        color: #fff;
        justify-content: center;
        align-items: center;
        display: flex;
        background: linear-gradient(to right top,rgba(0,0,0,.2),rgba(0,0,0,0));
    }
  }
}
</style>