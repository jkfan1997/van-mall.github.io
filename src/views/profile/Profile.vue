<template>
  <div>
    <nav-bar class='profile-nav'>
      <div slot='center'>我的</div>
    </nav-bar>
  </div>
</template>
<script>
import NavBar from "components/common/navbar/NavBar";

export default {
  name: "Profile",
  components: {
    NavBar
  }
};
</script>
<style scoped>
.profile-nav{
  background-color:var(--color-tint);
  color: #ffffff;
}
</style>