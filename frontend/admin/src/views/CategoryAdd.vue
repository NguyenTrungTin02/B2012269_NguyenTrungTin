<script>
import CategoryService from '../services/category.service';
import CategoryForm from '../components/CategoryForm.vue';
export default {
    components: {
        CategoryForm
    },
    data() {
        return {
            category: {
                name: "",
                imgCategory: ""
            },
            message: ""
        }
    },
    methods: {
        async createCategory(data) {

            try {
               
                await CategoryService.create(data);
                this.message = "Đã tạo 1 danh mục mới!";
                alert("Tạo thành công!!")
                this.$router.push('/categories');
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
                    <h2 style="text-align: center;">Thêm danh mục</h2>
                    <CategoryForm :category="category" @submit:category="createCategory" />
                    <p class="text-primary font-weight-bold">{{ message }}</p>
                </div>
            </div>
        </div>
    </div>
</template>