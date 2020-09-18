<template>
  <div class="home">
    <img
      alt="Vue logo"
      src="@/assets/logo.png"
    >
    <router-link
      class="btn btn-outline-primary"
      to="/login"
    >
      登入後台
    </router-link>  <!--透過路由轉換到login頁面-->
  </div>
</template>

<script>

export default {
  name: 'Home',
  data() {
    return {
    };
  },
};
</script>
