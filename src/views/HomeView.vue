<script setup>
import ProductCard from '@/components/ProductCard.vue';
import Pagination from '@/components/Pagination.vue';
import Loading from '@/components/Loading.vue';
import ProductForm from '@/components/ProductForm.vue';

import { onMounted ,ref, watch, watchEffect } from 'vue';
import axios from 'axios';

const products = ref([]);
const page = ref(1);
const limit = ref(8);
const isLoading = ref(true);

async function fetchData() {
	const API_URL = `http://localhost:3000/products?_page=${page.value}&_per_page=${limit.value}`;
	try {
		isLoading.value = true
		const response = await axios.get(API_URL);
		products.value = response.data;
	} catch(error) {
		console.log(error)
	} finally {
		isLoading.value = false
	}
}

watchEffect(() => {
	fetchData()
})

function changePage(newPage) {
	if (newPage < 1) return;
	if (newPage > products.value.pages) return;
	page.value = newPage
}

async function createProduct(product) {
	try {
		await axios.post("http://localhost:3000/products", product);
		fetchData();
	} catch (error) {
		console.log(error);
	}
}

// onMounted(async () => {
// 	try {
// 		products.value = await axios.get(API_URL).then((res) => res.data);
// 		onsole.log("first fetch")
// 	} catch (error) {
// 		console.log(error)
// 	} finally {
// 		isLoading.value = false
// 	}	
// });

//Suspense Componen
// products.value = await axios.get(`http://localhost:3000/products?_page=${page.value}&_per_page=${limit.value}`).then((res) => res.data);

// watch(page, async () => {
// 	try {
// 		isLoading.value = true;
// 		products.value = await axios
// 		.get(`http://localhost:3000/products?_page=${page.value}&_per_page=${limit.value}`)
// 		.then((res) => res.data);
// 		onsole.log("first fetch")
// 	} catch (error) {
// 		console.log(error)
// 	} finally {
// 		isLoading.value = false
// 	}
// })


//fetch data menggunakan axios dan async component
// async function getProducts() {
// 	const response = await axios.get("http://localhost:3000/products");
// 	products.value = response.data;
// 	console.log(response.data);
// }
// getProducts()

</script>

<template>
	<main>
		<div v-if="isLoading">
			<Loading />
		</div>
		<article v-else>
			<ProductForm @create-product="createProduct" />
			<div class="product-grid">
			<ProductCard v-for="(product, index) in products.data" :key="index" :product="product" />
			</div>

			<div class="pagination">
			<Pagination :page="page" :totalPages="products.pages" @change-page="changePage" />
			</div>
		</article>
	</main>
</template>

<style scoped>
.product-grid {
	display: grid;
	grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
	gap: 20px;
	width: 80%;
	margin: 0 auto;
}



.pagination {
	display: flex;
	justify-content: center;
	align-items: center;
	margin-top: 20px;
}


</style>