<script setup>
const props = defineProps({
	page: Number,
	totalPages: Number,
});

const emit = defineEmits(["change-page"]);

function changePage(newPage) {
	emit("change-page", newPage)
}
</script>

<template>
    <main>
        <button class="pagination-button" id="prevPage" @click="changePage(page - 1)" v-show="page > 1">&lt;</button>
		<span>Page {{ page }}</span>
		<button class="pagination-button" id="nextPage" @click="changePage(page + 1)" v-show="page < totalPages">&gt;</button>
    </main>
</template>

<style scoped>
.pagination-button {
	background-color: #fff;
	color: #ff0095;
	padding: 8px 12px;
	border: none;
	border-radius: 50%;
	cursor: pointer;
	font-size: 16px;
	margin: 0 5px;
}
</style>