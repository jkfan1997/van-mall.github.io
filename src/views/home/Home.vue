<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
      <tab-control 
      ref="tabControl1" 
      :titles="['流行','新款','精选']" 
      @tabClick="tabClick" 
      class='tab-control'
      v-show='isTabFixed'
      />
    <scroll
      class="content"
      ref="scroll"
      :probe-type="3"
      @scroll="contentScroll"
      :pull-up-load="true"
      @pullingUp="loadMore"
    >
      <home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad" />
      <recommend-view :recommends="recommends" />
      <feature-view />
      <tab-control 
      ref="tabControl2" 
      :titles="['流行','新款','精选']" 
      @tabClick="tabClick" 
      />
      <goods-list :goods="showGoods"></goods-list>
    </scroll>
    <back-top @click.native="backClick" v-show="isShowBackTop" />
  </div>
</template>
<script>
import NavBar from "components/common/navbar/NavBar";
import TabControl from "components/content/tabControl/TabControl";
import GoodsList from "components/content/goods/GoodsList";
import BackTop from "components/content/backTop/BackTop";
import Scroll from "components/common/scroll/Scroll";

import { debounce } from "../../common/utils";

import HomeSwiper from "./childComps/HomeSwiper";
import RecommendView from "./childComps/RecommendView";
import FeatureView from "./childComps/FeatureView";

import { getHomeMultiData, getHomeGoods } from "network/home";

export default {
  name: "Home",
  components: {
    NavBar,
    TabControl,
    GoodsList,
    BackTop,
    Scroll,
    HomeSwiper,
    RecommendView,
    FeatureView
  },
  data() {
    return {
      banners: [],
      recommends: [],
      goods: {
        pop: { page: 0, list: [] },
        new: { page: 0, list: [] },
        sell: { page: 0, list: [] }
      },
      carrentType: "pop",
      isShowBackTop: false,
      isTabFixed:false
    };
  },
  created() {
    // 1.请求多个数据
    this.getHomeMultiData(),
      // 2.请求商品类型
      this.getHomeGoods("pop");
    this.getHomeGoods("new");
    this.getHomeGoods("sell");
  },
  mounted() {
    // 1.监听item中图片加载完成
    const refresh = debounce(this.$refs.scroll.refresh, 200);
    this.$bus.$on("itemImageLoad", () => {
      refresh();
    });
  },
  computed: {
    showGoods() {
      return this.goods[this.carrentType].list;
    },
    destoryed(){
      console.log('xiaohui');
    },
     deactivated() {
      this.savaY= this.$refs.scroll.getScrollY()
    },
    activated() {
      this.$refs.scroll.scrollTo(0,this.saveY,0)
      this.$refs.scroll.refresh()
    },
   
  },
  methods: {
    // 事件监听相关方法
    tabClick(index) {
      switch (index) {
        case 0:
          this.carrentType = "pop";
          break;
        case 1:
          this.carrentType = "new";
          break;
        case 2:
          this.carrentType = "sell";
      }
      this.$refs.tabControl1.currentIndex = index
      this.$refs.tabControl2.currentIndex = index
    },
    // 网络请求相关方法
    getHomeMultiData() {
      getHomeMultiData().then(res => {
        // console.log(res);
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      });
    },
    getHomeGoods(type) {
      const page = this.goods[type].page + 1;
      getHomeGoods(type, page).then(res => {
        this.goods[type].list.push(...res.data.list);
        this.goods[type].page += 1;
        // console.log(res);
        this.$refs.scroll.finishPullUp();
      });
    },
    backClick() {
      this.$refs.scroll.scrollTo(0, 0);
    },
    contentScroll(position) {
      // 判断BackTop是否显示
      this.isShowBackTop = -position.y > 1000;
      // 决定tabControl是否吸顶（position:fixed）
      this.isTabFixed = -position.y > this.tabOffsetTop

    },
    loadMore() {
      this.getHomeGoods(this.carrentType);
    },

    swiperImageLoad() {
      this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop;
      console.log(this.tabOffsetTop);
    }
  }
};
</script>
<style scoped>
#home {
  /* padding-top: 44px; */
  height: 100vh;
  position: relative;
}
.home-nav {
  background-color: var(--color-tint);
  color: #ffffff;
  /* position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 9; */
}
/* .tab-control {
  position: sticky;
  top: 44px;
  z-index: 9;
} */
.content {
  position: absolute;
  /* background-color: red;
  height: calc(100% - 93px); */
  overflow: hidden;

  top: 44px;
  left: 0px;
  right: 0px;
  bottom: 49px;
}
.tab-control{
  position: relative;
  z-index: 9;
}
</style>