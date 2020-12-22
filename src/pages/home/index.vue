<template>
  <view>
    <view class="home_tab">
      <view class="home_tab_title">
        <view class="title_inner">
          <uni-segmented-control
            :values="items.map(v=>v.title)"
            :current="current"
            @clickItem="onClickItem"
            style-type="text"
            active-color="#d4237a"
          ></uni-segmented-control>
        </view>
        <view class="iconfont iconsearch"></view>
      </view>
    <view class="home_tab_content">
      <view v-if="current === 0">
        <HomeRecommend></HomeRecommend>
      </view>
      <view v-if="current === 1">
        <HomeCategory></HomeCategory>
      </view>
      <view v-if="current === 2">
        <HomeNew></HomeNew>
      </view>
      <view v-if="current === 3">
        <HomeAlbum></HomeAlbum>
      </view>
    </view>
  </view>
</view>
</template>

<script>
import HomeAlbum from "./home-album";
import HomeCategory from "./home-category";
import HomeNew from "./home-new";
import HomeRecommend from "./home-recommend";
export default {
  name: "",
  data() {
    return {
      items: [{ title: "推荐" }, { title: "分类" }, { title: "最新" }, { title: "专辑" }],
      current: 1,
    };
  },
  components: {
    HomeAlbum,
    HomeCategory,
    HomeNew,
    HomeRecommend,
  },
  methods: {
    onClickItem(e) {
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex;
      }
    },
  },
};
</script>

<style scoped lang="scss">
.home_tab{
    .home_tab_title{
        position: relative;
        .title_inner{
            width: 60%;
            margin: 0 auto;
        }
        .iconsearch{
            position: absolute;
            top: 50%;
            right: 5%;
            transform: translateY(-50%);
        }
    }
    .home_tab_content{

    }
}

</style>
