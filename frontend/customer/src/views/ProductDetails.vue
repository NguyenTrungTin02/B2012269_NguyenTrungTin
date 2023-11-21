<template>
	
	<div v-if="isLoading == false" class="section no-pad-bot" id="index-banner">
		
		<div class="container-fluid">
			<!-- <GoBack class="mb-2"/> -->
				<div class="">
					<GoBack class="me-auto"/>
					Chi tiết sản phẩm
				</div>
			<div class="row">
				<div class="col ">
					<img :src="getImageURL(product)" alt="" class="w-100">
				</div>

				<div class="col p-0">
					<div class="d-flex align-items-start flex-column h-100" >
						<div class="mb-auto p-2 " >
							<h2>{{ product.name }}</h2>
							<p>{{ product.description }}</p>
							<p>Còn lại: <span class="text-danger fw-semibold">{{ product.itemsInStock }}</span> sản phẩm</p>
							<h3 class="text-primary">{{ product.price.toLocaleString('vi-VN', {style: 'currency', currency: 'VND'}) }}</h3>
							<AddToCart :product="product" @productAdded="onProductAdded"/>
						</div>
						<div class="w-100 rounded-1 text-center" style="background-color: #fada91;">
							<span class="text-center fw-bold" style="color: #f09605; ">Bạn đã thêm {{ itemsInCart }} sản phẩm vào giỏ hàng</span>
						</div>
					</div>
					
				</div>
			</div>
		</div>
		<hr>
		<div class="container">
			<div class="row mt-1">
				<div class="bg-light p-2">
						<h5>THÔNG TIN CHI TIẾT</h5>
					</div>
				<div class="col">
					
					

					<div class="col">
						<table class="table">
							<tr v-for="detail in product.subDescription">
								<th class="w-50 p-1">{{ detail.key }}</th>
								<td>{{ detail.value }}</td>
							</tr>
						</table>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import { useCartStore } from '../stores/CartStore';
import ProductService from '../services/product.service';
import AddToCart from '../components/AddToCart.vue';
import GoBack from '../components/GoBack.vue';
import CategoryService from '../services/category.service';
import ProductList from '../components/ProductList.vue';
export default {
	components: {
		AddToCart,
		GoBack,
		ProductList
	},
	data() {
		const useCart = useCartStore();
		return {
			
			product: {},
			isLoading: true,
			suggestProducts: [],
			useCart,
            itemsInCart: 0
		}
	},
	watch: {
		'$route.params.id': {
			immediate: true,
			handler(val) {
				this.getProduct();
				this.isLoading = true;
			}
		},
		isLoading (value) {
			if (!value) {
				this.getItemsInCart(this.product);
			}
		}
	},
	
	methods: {
		async getProduct() {
			try {
				this.product = await ProductService.get(this.$route.params.id);
				// console.log(this.product.categoryId);
				await this.retriveProduct(this.product.categoryId);
				this.isLoading = false;
				
			} catch (error) {
				console.log(error);
			}
		},
		getImageURL(item) {
            return `http://localhost:9000/${item.image}`;
        },
		async retriveProduct(id) {
            try {
                const pros = await CategoryService.get(id);
				// console.log(pros);
				// this.suggestProducts = pros.products;
                this.suggestProducts = pros.products.filter((pro) => pro._id != this.product._id);
                // console.log(this.suggestProducts);
            } catch (error) {
                console.log(error);
            }
        },
		onProductAdded(quantity) {
            this.itemsInCart += quantity;
            console.log(this.itemsInCart);
        },
		getItemsInCart(product) {
			if (this.isLoading == false) {
				const productInCart = this.useCart.cart.items.find(item => item._id === product._id);
            
				if (productInCart) {
					this.itemsInCart = productInCart.quantity;
				} else {
					this.itemsInCart = 0;
				}
				console.log(this.itemsInCart);
			} else {
				console.log("none");
			}
            
        },
	},
	mounted () {
		this.getProduct();
		// this.getItemsInCart(this.product);
	}
}

</script>

