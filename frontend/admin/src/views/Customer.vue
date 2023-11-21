<template>
    <div class="row">
      <div class="col-md-12">
        <div class="card m-2 border-0">
          <div class="card-header border-0 bg-white">
            <h4 class="font-weight-bolder text-uppercase text-primary opacity-7 ">Danh mục khách hàng</h4>
            <div class="row">
              <div class="col p-1"></div>
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
                    <th class="text-uppercase text-secondary opacity-7 text-center" style="background-color: aquamarine;">Họ và tên</th>
                    <th class="text-uppercase text-secondary opacity-7 text-center" style="background-color: aquamarine;">Email</th>
                    <th class="text-uppercase text-secondary opacity-7 text-center" style="background-color: aquamarine;">SĐT</th>
                    <th class="text-uppercase text-secondary opacity-7 text-center" style="background-color: aquamarine;">Địa chỉ</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-if="filteredDataCount > 0" v-for="(customer, index) in filteredData" :key="customer._id">
                    <td>{{ ++index }}</td>
                    <td>{{ customer.fullname }}</td>
                    <td>{{ customer.email }}</td>
                    <td>{{ customer.phone }}</td>
                    <td>{{ customer.address }}</td>
                  </tr>
                  <tr v-else>
                    <td colspan="5" class="text-center fst-italic fw-bolder">
                      Không tìm thấy dữ liệu phù hợp...
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  

<script>
import InputSearch from '../components/InputSearch.vue';
import CustomerService from '../services/customer.service';
export default {
    components: {
        InputSearch,
    },
    data () {
        return {
            customers: [],
            searchText: ""
        };
    },
    computed: {
            customerStrings() {
                return this.customers.map((customer) => {
                    const { fullname, email, phone, address } = customer;
                    return [fullname, email, phone, address].join("");
                });
            },
            filteredData() {
                if (!this.searchText)
                    return this.customers;
                return this.customers.filter((customer, index) => {
                    return this.customerStrings[index].includes(this.searchText);
                });
            },
            filteredDataCount() {
                return this.filteredData.length;
            },
        },
    methods: {
        async retriveCategory () {
            try {
                this.customers = await CustomerService.getAll();
            } catch (error) {
                console.log(error);
            }
        },
        
        refreshList() {
            this.retriveCategory();
           
        },
      
     },
    mounted () {
        this.refreshList();
    }
}
</script>
