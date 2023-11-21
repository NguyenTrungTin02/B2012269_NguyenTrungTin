<script>

import { useCartStore } from '../stores/CartStore';
import AddToCart from './AddToCart.vue';
export default {

    data() {
        const useCart = useCartStore();
        return {
            useCart,
            itemsInCart: 0
        }
    },

    props: {
        product: {
            type: Object,
            required: true
        }
    },
    components: {
        AddToCart,
    },
    
    methods: {

        getItemsInCart(product) {
            const productInCart = this.useCart.cart.items.find(item => item._id === product._id);
            // console.log("yes");
            // console.log(this.useCart.cart.items);
            if (productInCart) {
                this.itemsInCart = productInCart.quantity;
            } else {
                this.itemsInCart = 0;
            }
        },
        goToDetail(productId) {
            this.$router.push({ name: "product.detail", params: { id: productId } });
        },
        getImageURL(item) {
            return `http://localhost:9000/${item.image}`;
        },
        onProductAdded(quantity) {
            this.itemsInCart += quantity;
            console.log(this.itemsInCart);
        }
    },
    mounted() {
        this.getItemsInCart(this.product);
    }
};
</script>
<template>
    <div class="card shadow m-2 p-0 border-0" @click="goToDetail(product._id)" style="background-color: #fada91; width: 200px; height: 300px;">
      <div>
        <img :src="getImageURL(product)" class="card-img-top" alt="Product Image">
      </div>
      <div class="card-footer py-0 px-1 border-0 text-center" style="">
        <span class="text-center fw-bold" style="color: #f09605; ">Đã thêm {{ itemsInCart }} sản phẩm vào giỏ hàng</span>
      </div>
      <div class="card-body bg-white rounded-bottom pb-0">
        <h3 class="card-title">{{ product.name }}</h3>
        <p class="card-text">Còn lại: <span class="fst-italic text-success fw-bold">{{ product.itemsInStock }}</span> sản phẩm</p>
        <h4 class="text-uppercase text-primary fw-bold"><strong></strong>{{ product.price.toLocaleString('vi-VN', {
            style: 'currency',
            currency: 'VND'
          }) }}
        </h4>
        <div class="d-flex flex-column align-items-center" @click.stop="">
         
          <AddToCart :product="product" @productAdded="onProductAdded"/>
        
         
        </div>
        
      </div>
      
    </div>
  </template>

