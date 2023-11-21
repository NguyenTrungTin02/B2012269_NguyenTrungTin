<template>
    <header class="fixed-top">
        <div class="container-lg bg-white">
            
            <nav class="navbar navbar-expand-md navbar-light "  >
                <a class="navbar-brand" href="">SHOPE BÁN ĐỒ CÔNG NGHỆ</a>

                <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNavDropdown"
                        aria-controls="navbarNavDropdown" aria-expanded="false" aria-label="Toggle navigation">
                    <span class="navbar-toggler-icon"></span>
                </button>
                <div class="collapse navbar-collapse" id="navbarNavDropdown">
                    <ul class="navbar-nav ms-auto">
                        <li class="nav-item">
                            <router-link to="/" class="nav-link">Trang chủ</router-link>
                        </li>
                        <template v-if="isAuth">
                                <li class="nav-item dropdown">
                                <router-link to="/order" class="nav-link">Đơn hàng</router-link>
                                </li>

                                <li class="nav-item dropdown ">
                                <router-link to="/cart" class="nav-link position-relative">
                                        Giỏ hàng
                                        <span class="position-absolute top-1 start-100 translate-middle badge rounded-pill bg-danger" style="font-size: small;">{{ cartCount }}</span>
                                </router-link>
                            </li>
                            
                                <li class="nav-item dropdown">
                                    <sapn class="nav-link text-primary fw-bold" @click="logout" style="cursor: pointer;">Đăng xuất</sapn>
                                
                                </li>
                            
                               
                        </template>
                        <template v-else>
                            <li class="nav-item dropdown ">
                                <router-link to="/login" class="nav-link">Đăng nhập</router-link>
                            </li>
                                    <li class="nav-item dropdown ">
                                   
                                    <router-link to="/register" class="nav-link">Đăng ký</router-link>
                                    </li>
                        </template>
                        
                    </ul>
                </div>
            </nav>
        </div>
    </header>
</template>

<script>
import { useCartStore } from '@/stores/CartStore';
import { mapActions, mapState} from "pinia";
import { useAuthStore } from "@/stores/AuthStore";
export default {
    methods: {
        ...mapActions(useAuthStore, {
            logout: "logout"
        }),
        
    },
    computed: {
        ...mapState(useAuthStore, {
            isAuth: 'isAuth',
        }),
        
        ...mapState(useCartStore, ['cart']),
        cartCount() {
            return this.cart.items.length;
        }
    }
}
</script>

<style scoped>
    .form-control:focus {
        border-color: #ced4da;
        box-shadow: none;
    }

    .btn-outline-secondary:hover {
        background-color: #f8f9fa;
    }

    .input-group-append button {
        border-color: #ced4da;
    }

    .input-group-append button:hover {
        background-color: #f8f9fa;
    }
</style>