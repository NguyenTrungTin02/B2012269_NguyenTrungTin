<script>
import ProductService from '../services/product.service';
import ProductForm from '../components/ProductForm.vue';
export default {
    components: {
        ProductForm
    },
    data() {
        return {
            product: null,
            message: ""
        }
    },
    methods: {
        async createProduct(data) {
            try {
                await ProductService.create(data);
                this.message = "Đã tạo 1 sản phẩm mới!";
                alert("Tạo thành công!!")
                this.$router.push('/products');
            }
            catch (err) {
                console.log(err);
            }
        }
    }
}
</script>

<template >
    <div class="row">
        <div class="col-md-12">
            <div class="card m-2 border-0">
                <div class="card-body">
                    <h2 style="text-align: center;">Thêm sản phẩm</h2>
                    <ProductForm :product="{}" @submit:product="createProduct" />
                    <p class="text-primary font-weight-bold">{{ message }}</p>
                </div>
            </div>
        </div>
    </div>
</template>