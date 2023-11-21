<template>
    <div class="row">
      <div class="col-md-12">
        <div class="card m-2 border-0">
          <div class="card-header border-0 bg-white">
            <h4 class="font-weight-bolder text-uppercase text-primary opacity-7 ">Danh mục sản phẩm</h4>
            <div class="row">
              <div class="col">
                <InputSearch v-model="searchText" class="mt-2" />
              </div>
            </div>
          </div>
  
          <div class="card-body">
            <div class="table-responsive p-0">
              <table class="table table-bordered table-striped align-items-center justify-content-center mb-0" id="categories-table">
                <thead>
                  <tr>
                    <th class="text-uppercase text-secondary opacity-7 text-center" style="background-color: aquamarine;" >STT</th>
                    <th class="text-uppercase text-secondary opacity-7 text-center" style="background-color: aquamarine;">Hình ảnh</th>
                    <th class="text-uppercase text-secondary opacity-7 text-center" style="background-color: aquamarine;">Tên</th>
                    <th class="text-uppercase text-secondary opacity-7 text-center" style="background-color: aquamarine;">Thao tác</th>
                  </tr>
                </thead>
  
                <tbody>
                  <tr v-if="filteredDataCount > 0" v-for="(category, index) in filteredData" :key="category._id">
                    <td style="text-align: center;">{{ ++index }}</td>
                    <td style="text-align: center;">
                      <img :src="getImageURL(category)" alt="" style="width: 100px; height: 100px;">
                    </td>
                    <td style="text-align: center;">{{ category.name }}</td>
  
                    <td style="text-align: center;">
                      <router-link
                        :to="{ name: 'category.edit', params: { id: category._id } }"
                        class="btn btn-warning me-1 text-white"
                      >
                        <i class="fa-solid fa-pen-to-square"></i>
                      </router-link>
  
                      <button class="btn btn-danger" @click="deleteCategory(category._id)">
                        <i class="fa fa-trash"></i>
                      </button>
                    </td>
                  </tr>
                  <tr v-else>
                    <td colspan="4" class="text-center fst-italic fw-bolder">
                      Không tìm thấy dữ liệu phù hợp...
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
          <div class="col p-1">
                <router-link to="/categories/add" class="btn btn-outline-primary float-end" >
                   Thêm danh mục
                </router-link>
              </div>
        </div>
        
      </div>
    </div>
  </template>
  

<script>
import InputSearch from '../components/InputSearch.vue';
import CategoryService from '../services/category.service';
export default {
    components: {
        InputSearch,
    },
    data() {
        return {
            categories: [],
            searchText: ""
        };
    },
    computed: {
        categoryStrings() {
            return this.categories.map((category) => {
                const { name } = category;
                return [name].join("");
            });
        },
        filteredData() {
            if (!this.searchText)
                return this.categories;
            return this.categories.filter((category, index) => {
                return this.categoryStrings[index].includes(this.searchText);
            });
        },
        filteredDataCount() {
            return this.filteredData.length;
        },
    },
    methods: {
        async retriveCategory() {
            try {
                this.categories = await CategoryService.getAll();
            } catch (error) {
                console.log(error);
            }
        },
        async deleteCategory(id) {
            if (confirm("Bạn chắc chắn muốn xóa danh mục này?")) {
                try {
                    location.reload();
                    await CategoryService.delete(id);

                    this.retriveCategory();
                    
                } catch (error) {
                    console.log(error);
                }
            }
        },

        refreshList() {
            this.retriveCategory();

        },
        getImageURL(category) {
            return `http://localhost:9000/${category.image}`;
        }
    },
    mounted() {
        this.refreshList();
    }
}
</script>
