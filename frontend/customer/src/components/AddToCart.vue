<template>
  <template v-if="isAuth">
    <div class="mb-3">
      <div class="d-flex justify-content-between align-items-center border rounded p-1 mb-2" style="max-width: 120px;">
        <button class="btn btn-outline-primary btn-sm" @click="decrement" :disabled="quantity <= 0">-</button>
        <span class="h5 mx-2">{{ quantity }}</span>
        <button class="btn btn-outline-primary btn-sm" @click="increment">+</button>
      </div>
      <button class="btn btn-primary" @click="addToCart(product._id, quantity)" :disabled="CanAdd" style="width: 100%;">
        Thêm vào giỏ hàng
      </button>
    </div>
  </template>
  <template v-else>
    <div>
      <!-- Content for when the user is not authenticated -->
    </div>
  </template>
</template>





  <script>
      import { mapState } from 'pinia';
      import { useCartStore } from '../stores/CartStore';
      import { useAuthStore } from '../stores/AuthStore';
    export default {
      props: {
        product: {
          type: Object,
          required: true
        }
      },
      data() {
        return {
          quantity: 0,
          maxItems: 0
        };
      },
      emits: ['productAdded'],
      methods: {
        increment() {
          if (this.quantity < this.maxItems)
            this.quantity++;
        },
        decrement() {
          if (this.quantity > 0)
            this.quantity--;
        },
        addToCart(productId, quantity) {
          // addToCart(productId, quantity) {
          if(confirm("Bạn muốn thêm " + quantity + " vào giỏ hàng? ")){
            const cartStore = useCartStore();
            cartStore.addToCart(productId, quantity);
            // cartStore.getItemsInCart();
            // this.getItemsInCart(this.product);
            this.maxItems -= quantity;
  
            this.$emit('productAdded', quantity);
            this.quantity = 0;
          }
  
        },
        getItemsInCart(product){
          const productInCart = this.cart.items.find(item => item._id === product._id);
          if (productInCart) {
            this.maxItems = product.itemsInStock - productInCart.quantity;
          } else {
              this.maxItems = product.itemsInStock;
          }
        }
      },
      computed: {
        ...mapState(useCartStore, ['cart']),
        ...mapState(useAuthStore, {
              isAuth: 'isAuth',
          }),
        CanAdd() {
          if (this.quantity == 0) {
            return true;
          }
          return false;
        }
   
      },
      mounted () {
        this.maxItems = this.product.itemsInStock;
        // console.log(this.maxItems);
        this.getItemsInCart(this.product);
      }
    };
  </script>