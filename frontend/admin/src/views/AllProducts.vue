<template>
    <div class="row">
      <div class="col-md-12">
        <div class="card m-2 border-0">
          <div class="card-header border-0 bg-white">
            <h4 class="font-weight-bolder text-uppercase text-primary opacity-7 ">Quản lý sản phẩm</h4>
            <div class="row">
              <div class="col">
                <InputSearch v-model="searchText" class="mt-2" />
              </div>
            </div>
          </div>
  
          <div class="card-body">
            <div class="table-responsive p-0">
              <table class="table table-bordered table-striped align-items-center justify-content-center mb-0" id="products-table">
                <thead>
                  <tr>
                    <th class="text-uppercase text-secondary opacity-7 text-center" style="background-color: aquamarine;">STT</th>
                    <th class="text-uppercase text-secondary opacity-7 text-center" style="background-color: aquamarine;">Hình ảnh</th>
                    <th class="text-uppercase text-secondary opacity-7 text-center" style="background-color: aquamarine;">Tên sản phẩm</th>
                    <th class="text-uppercase text-secondary opacity-7 text-center" style="background-color: aquamarine;">Giá</th>
                    <th class="text-uppercase text-secondary opacity-7 text-center" style="background-color: aquamarine;">Số lượng</th>
                    <th class="text-uppercase text-secondary opacity-7 text-center" style="background-color: aquamarine;">Thao tác</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-if="filteredDataCount > 0" v-for="(product, index) in filteredData" :key="product._id">
                    <td style="text-align: center;">{{ ++index }}</td>
                    <td style="text-align: center;">
                      <img :src="getImageURL(product)" alt="" style="width: 150px; height: 150px;">
                    </td>
                    <td style="text-align: center;">{{ product.name }}</td>
                    <td style="text-align: center;">{{ product.price.toLocaleString('vi-VN', {style: 'currency', currency: 'VND'}) }}</td>
                    <td style="text-align: center;">{{ product.itemsInStock }}</td>
                    <td style="text-align: center;">
                      <router-link :to="{ name: 'product.edit', params: { id: product._id } }" class="btn btn-warning me-1 text-white">
                        <i class="fa-solid fa-pen-to-square"></i>
                      </router-link>
                      <button class="btn btn-danger" @click="deleteProduct(product._id)">
                        <i class="fa fa-trash"></i>
                      </button>
                    </td>
                  </tr>
                  <tr v-else>
                    <td colspan="6" class="text-center fst-italic fw-bolder">
                      Không tìm thấy dữ liệu phù hợp...
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
  
          <div class="col p-1">
            <router-link to="/products/add" class="btn btn-outline-primary float-end"> Thêm sản phẩm</router-link>
          </div>
        </div>
      </div>
    </div>
  </template>
  

<script>
import InputSearch from '../components/InputSearch.vue';
import ProductService from '../services/product.service';
export default {
    components: {
        InputSearch,
    },
    data() {
        return {
            products: [],
            searchText: ""
        };
    },
    computed: {
        productStrings() {
            return this.products.map((product) => {
                const { name, price, itemsInStock } = product;
                return [name, price, itemsInStock].join("");
            });
        },
        filteredData() {
            if (!this.searchText)
                return this.products;
            return this.products.filter((product, index) => {
                return this.productStrings[index].includes(this.searchText);
            });
        },
        filteredDataCount() {
            return this.filteredData.length;
        },
    },
    methods: {
        async retriveProduct() {
            try {
                this.products = await ProductService.getAll();
            } catch (error) {
                console.log(error);
            }
        },
        async deleteProduct(id) {
            if (confirm("Bạn chắc chắn muốn xóa sản phẩm này?")) {
                try {
                    location.reload();
                    await ProductService.delete(id);

                    this.retriveProduct();
                    
                } catch (error) {
                    console.log(error);
                }
            }
        },
        refreshList() {
            this.retriveProduct();

        },
        getImageURL(product) {
            return `http://localhost:9000/${product.image}`;
        }
    },
    mounted() {
        this.refreshList();
    }
}
</script>
