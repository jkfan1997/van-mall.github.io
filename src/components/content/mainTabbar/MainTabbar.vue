<template>
  <tab-bar>
    <tab-bar-Item path="/home">
      <img slot="item-icon" src="~/assets/img/tabbar/home.svg" alt />
      <img slot="item-icon-active" src="~/assets/img/tabbar/home_active.svg" alt />
      <div slot="item-text">首页</div>
    </tab-bar-Item>
    <tab-bar-Item path="/category">
      <img slot="item-icon" src="~/assets/img/tabbar/category.svg" alt />
      <img slot="item-icon-active" src="~/assets/img/tabbar/category_active.svg" alt />
      <div slot="item-text">分类</div>
    </tab-bar-Item>
    <tab-bar-Item path="/cart">
      <img slot="item-icon" src="~/assets/img/tabbar/category.svg" alt />
      <img slot="item-icon-active" src="~/assets/img/tabbar/cart_active.svg" alt />
      <div slot="item-text">购物车</div>
    </tab-bar-Item>
    <tab-bar-Item path="/profile">
      <img slot="item-icon" src="~/assets/img/tabbar/profile.svg" alt />
      <img slot="item-icon-active" src="~/assets/img/tabbar/profile_active.svg" alt />
      <div slot="item-text">我的</div>
    </tab-bar-Item>
  </tab-bar>
</template>
<script>
import TabBarItem from "components/common/tabbar/TabBarItem";
import TabBar from "components/common/tabbar/TabBar";
export default {
  name: "MainTabBar",
  components: {
    TabBarItem,
    TabBar
  }
};
</script>
<style  scoped></style>