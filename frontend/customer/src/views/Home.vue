<template>
  <div style="padding-top: 20px;"><InputSearch v-model="searchText" class="mt-2"/></div>
  <div class="row p-2 bg-light my-2">
    <h5>Danh mục sản phẩm</h5>
  </div>
  <CategoryList/>
  <div class="row p-2 bg-light my-2">
    <h5></h5>
  </div>
</template>

<script>
import { mapActions } from 'pinia';
import { useCartStore } from '@/stores/CartStore';
import ProductList from '../components/ProductList.vue';
import CategoryList from '../components/CategoryList.vue';
import ProductService from '../services/product.service';
import InputSearch from '../components/InputSearch.vue';
export default {
  components: {
    ProductList,
    CategoryList,
    InputSearch
  },
  data() {
    return {
      products: [],
      searchText: "",
    }
  },
 
  computed: {
    productStrings() {
      return this.products.map((product) => {
        const { name, description, price } = product;
        return [name, description, price].join("");
      });
    },
    filteredProducts() {
      if (!this.searchText)
        return this.products;
      return this.products.filter((_product, index) => {
        return this.productStrings[index].includes(this.searchText);
      });
    },
    filteredProductsCount() {
      return this.filteredProducts.length;
    },
    ...mapActions(useCartStore, {
      getItemsInCart: "getItemsInCart"
    })
  },
  methods: {
    async retriveProduct() {
      try {
        this.products = await ProductService.getAll();
        console.log(this.products);
      } catch (error) {
        console.log(error);
      }
    },
    refreshList() {
      this.retriveProduct();
    },


  },
  mounted() {
    this.refreshList()
    // this.getItemsInCart()
  }
}
</script>

<style scoped>
/* .slide {
  box-shadow: 0 6px 15px -3px rgb(0 0 0/0.3);
  padding: 25px;
  border-radius: 5px;
} */
.carousel-item img {
  height: 400px;
}
</style>