<template>
    <div class="table-responsive p-0">
        <table class="table align-items-center justify-content-center mb-0" id="products-table">
            <thead>
                <tr>
                    <th class="text-uppercase text-secondary opacity-7">Mã đơn</th>
                   
                    <th class="text-uppercase text-secondary opacity-7">Tổng tiền</th>
                    <th class="text-uppercase text-secondary opacity-7">Ngày đặt</th>
                    <th class="text-uppercase text-secondary opacity-7">Trạng thái</th>
                </tr>
            </thead>

            <tbody>
                <tr v-if="orders" v-for="(order, index) in orders" :key="order._id">
                    <td>
                        {{ order._id }}
                    </td>
                    
                    <td>
                        {{ order.total.toLocaleString('vi-VN', { style: 'currency', currency: 'VND' }) }}
                    </td>
                    <td>
                        {{ formatDate(order.createdAt )}}
                    </td>
                    <td>
                        <span v-if="order.status == 0" class="font-italic">Chờ xác nhận...</span>
                        <span v-else-if="order.status == 1" class="font-italic font-weight-bold">Đã xác nhận</span>
                        <span v-else-if="order.status == -1" class="font-italic font-weight-bold">Đã hủy</span>
                    </td>
                </tr>
                <tr v-else>
                    <td colspan="9" class="text-center fst-italic fw-bolder">
                        Chưa có đơn hàng nào...
                    </td>

                </tr>
            </tbody>
        </table>
               
    </div>
</template>

<script>
import moment from 'moment';
import OrderService from '../services/order.service';
export default {
    data() {
        return {
            orders: [],
            orderDetail: {
                name: "",
                email: "",
                phone: "",
                address: "",
                payment: "",
                message: "",
                details: [],
                total: 0
            },
        }
    },
    
    methods: {
        async getOrder() {
            try {
                const data = await OrderService.personalOrder();
                console.log(data);
                return this.orders = data;
            } catch (error) {
                console.log(error)
            }
        },
        async cancel (id) {
            if(confirm("Bạn chắc chắn muốn hủy đơn hàng này! Sau khi đồng ý trạng thái đơn hàng không thể khôi phục.")){
                try {
                    await OrderService.execute(id, {status: -1});
                    this.getOrder();
                    console.log("getOrder");
                } catch (error) {
                    console.log(error);
                }
            }
        },
        fillDetails (id) {
            return this.orderDetail = this.orders.find((order) => order._id === id);
        },
        formatDate (time) {
            return time.substring(0, 10);
        }
    },
    filters: {
        moment: function (date) {
            return moment(date).format('MMMM Do YYYY, h:mm:ss a');
        }
    },
    mounted() {
        this.getOrder();
    }
}
</script>