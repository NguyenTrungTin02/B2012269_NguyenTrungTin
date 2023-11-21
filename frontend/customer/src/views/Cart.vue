<template>
  
  <h2 class="mt-2">Giỏ hàng của bạn</h2>
  <div v-if="cart.items.length == 0" class="text-center">
    Giỏ hàng trống
  </div>
  <div v-else class="row ">

    <div class="col-12 col-lg-12 col-sm-12">
      <div class="order_div">
        <h2>Đơn hàng</h2>
        <table class="table">
          <tr v-for="item in cart.items" :key="item.productId">
            <th class="text-start">
              {{ item.name }}
            </th>
            <td class="text-end">
              {{ item.price.toLocaleString('vi-VN', {style: 'currency', currency: 'VND'})  }} x {{ item.quantity }}
            </td>
          </tr>
          <tr>
            <th class="text-start">
              Tổng cộng
            </th>
            <td class="text-end">
              {{ cart.total.toLocaleString('vi-VN', {style: 'currency', currency: 'VND'})  }}
            </td>
          </tr>
        </table>
        <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#staticBackdrop" @click="fillData">ĐẶT HÀNG</button>
        <!-- Modal -->
        <div class="modal modal-lg fade" id="staticBackdrop" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
          <div class="modal-dialog">
            <div class="modal-content">
              <div class="modal-header">
                <h1 class="modal-title fs-5" id="staticBackdropLabel">Đơn đặt hàng</h1>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
              </div>
              <div class="modal-body">
                <h5>Thông tin đơn hàng</h5>
                <table class="table table-striped">
                  <tr v-for="item in cart.items" :key="item.productId">
                    <th class="text-start">
                      {{ item.name }}
                    </th>
                    <td class="text-end">
                      {{ item.price.toLocaleString('vi-VN', {style: 'currency', currency: 'VND'})  }} x {{ item.quantity }} = {{ (item.price * item.quantity).toLocaleString('vi-VN', {style: 'currency', currency: 'VND'})  }}
                    </td>
                  </tr>
                  <tr>
                    <th class="text-start">
                      TỔNG CỘNG
                    </th>
                    <td class="text-end ">
                      {{ cart.total.toLocaleString('vi-VN', {style: 'currency', currency: 'VND'})  }}
                    </td>
                  </tr>
                </table>
                <hr>
                  <div class="form-group m-1">
                        <button @click="submitOrder" class="btn btn-primary w-100" data-bs-dismiss="modal">
                            Xác nhận đặt hàng
                        </button>
                    </div>
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Thoát</button>
              </div>
            
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapActions } from 'pinia';
import { useCartStore } from '../stores/CartStore';
import CartService from '../services/cart.service';
import OrderService from '../services/order.service';
import CustomerService from "../services/customer.service";
import * as yup from "yup";
import { Form, Field, ErrorMessage } from 'vee-validate';
export default {
  components: {
    Form,
    Field,
    ErrorMessage
  },
  data () {
    const orderFormSchema = yup.object().shape({
      fullname: yup
        .string()
        .required("Tên phải có giá trị.")
        .min(2, "Tên phải có ít nhất 2 ký tự.")
        .max(50, "Tên có nhiều hơn 50 ký tự."),
      email: yup
        .string()
        .required("Chưa nhập địa chỉ email.")
        .email("E-mail không đúng.")
        .max(50, "email chỉ chứa tối đa 50 ký tự."),
      address: yup
        .string()
        .max(255, "Địa chỉ chỉ chứa tối đa 255 ký tự."),
      phone: yup
        .string()
        .matches(
          /((09|03|07|08|05)+([0-9]{8})\b)/g,
          "Số điện thoại không hợp lệ."
        ),
      
    });
    return {
      customer: null,
      orderLocal: [],
      orderFormSchema,
    }
  },
  methods: {
    ...mapActions(useCartStore, {
      getItemsInCart: "getItemsInCart",
    }),
    async removeFromCart(productId) {
      try {
        console.log(productId)
        const data = {
          productId: productId
        }
        await CartService.removeFromCart(data);
        this.getItemsInCart();
        console.log("remove from cart");
      } catch (error) {
        console.log(error);
      }
    },
    async updateItemInCart(productId, quantity) {
      try {
        const data = {
          productId,
          quantity
        }
        await CartService.updateCart(data);
        this.getItemsInCart();

      } catch (error) {
        console.log(error);
      }
    },
    async getInforUser() {
      try {
        const document = await CustomerService.profile();
        return this.customer = document;
      } catch (error) {
        console.log(error);
      }
    },
    async submitOrder () {
      try {
        const data = {
          name: this.orderLocal.name,
          email: this.orderLocal.email,
          phone: this.orderLocal.phone,
          message: this.orderLocal.message,
          address: this.orderLocal.address,
          payment: "TM"
        }
        await OrderService.create(data);
        this.getItemsInCart();
        alert("Đặt hàng thành công!");
        console.log("add order");
      } catch (error) {
        console.log(error)
      }
    },
    fillData () {
      this.orderLocal = this.customer;
      this.orderLocal.name = this.customer.fullname;
      return this.orderLocal;
    }
  },
  computed: {
    cart() {
      return useCartStore().cart;
    }
  },
 
  mounted() {
    this.getItemsInCart();
    this.getInforUser();
  }


};
</script>

<style scoped>

section {
  margin: 2rem auto;
  max-width: 40rem;
}

h2 {
  color: #292929;
  text-align: center;
  border-bottom: 2px solid #ccc;
  padding-bottom: 1rem;
}

h3 {
  text-align: center;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

.order_div {
  margin: 1rem auto;
  padding: 1rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  text-align: center;
  /* max-width: 50rem; */
}



</style>