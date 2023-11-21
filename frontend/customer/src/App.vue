<script>
import AppHeader from './components/AppHeader.vue';
import { useAuthStore } from './stores/AuthStore';
import { useCartStore } from './stores/CartStore';
import Cookies from "js-cookie";
import { mapActions } from 'pinia';
export default {
  components: {
    AppHeader,

  }, 
  data() {
    const authStore = useAuthStore();
    return {
      authStore,
    }
  },
  computed: {
    isLoggedIn () {
      if(this.authStore.isAuth){
        this.getItemsInCart();
      }
    }
  },
  methods: {
    tryLogin() {
      const tokenFromCookies = Cookies.get("token");
      if (tokenFromCookies) {
        this.authStore.token = tokenFromCookies;
        this.authStore.isAuth = true;
        this.getItemsInCart();
      } else {
        this.authStore.token = "";
        this.authStore.isAuth = false;
      }
      
    },
    ...mapActions(useCartStore, {
      getItemsInCart: "getItemsInCart"
    })
  },
  created () {
    this.tryLogin();
    
  }
}
</script>

<template>
  <AppHeader />
  <main class="container">
    <router-view></router-view>
  </main>
</template>

<style scoped>

</style>
