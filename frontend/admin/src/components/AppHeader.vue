<template>
  <nav class="navbar navbar-expand navbar-dark bg-dark" id="headerI" v-if="isAuth" >
    <a href="#" class="navbar-brand" id="nameShope">QUẢN LÝ CỬA HÀNG</a>
      <div class="mr-auto navbar-nav">
        <li class="nav-item " :class="{ 'active': $route.path.match(/^\/categories/) }">
                <router-link to="/categories" class="nav-link" >Danh mục sản phẩm
                </router-link>
          </li>
          <li class="nav-item " :class="{ 'active': $route.path.match(/^\/products/) }">
                <router-link to="/products" class="nav-link">Quản lý sản phẩm
                </router-link>
          </li>
          <li class="nav-item " :class="{ 'active': $route.path.match(/^\/orders/) }">
                <router-link to="/orders" class="nav-link">
                    Quản lý đặt hàng
                </router-link >
          </li>
          <li class="nav-item " :class="{ 'active': $route.path.match(/^\/customers/) }">
                <router-link to="/customers" class="nav-link">
                    Danh sách khách hàng
                </router-link >
                
          </li>



          <li class="nav-item">
              <button id="logout" href="" @click="logout">Đăng xuất</button>
          </li>


        
        </div>
      </nav>
</template>





  <script>
  import { mapActions, mapState } from "pinia";
  import { useAdminAuthStore } from "../stores/AdminAuthStore";
  export default {
    methods: {
      toggleSidebar() {
        const sidebar = document.querySelector('#sidebar');
        const content = document.querySelector('#content');
        sidebar.classList.toggle('active');
        content.classList.toggle('active');
      },
        ...mapActions(useAdminAuthStore, {
            logout: "logout"
        })
      
    },
    computed: {
          ...mapState(useAdminAuthStore, {
              isAuth: 'isAuth',
          }),
      }
  }
  </script>