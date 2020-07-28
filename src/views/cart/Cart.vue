<template>
  <div>
    <nav-bar class='cart-nav'>
      <div slot='center'>购物车 ({{num}})</div>
    </nav-bar>
  </div>
</template>
<script>
import NavBar from 'components/common/navbar/NavBar'
export default {
  data(){
    return{
      num:0
    }
  },
  components: {
    NavBar,
  },
  name: "Cart"
};
</script>
<style scoped>
  .cart-nav{
    background-color: var(--color-tint);
    color: #ffffff;
  }
</style>