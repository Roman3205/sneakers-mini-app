<template>
  <div>
    <the-search-bar
      v-model:searchQuery="filters.searchQuery"
      v-model:sortBy="filters.sortBy"
    ></the-search-bar>
    <the-products-grid-vue :products="products"></the-products-grid-vue>
  </div>
</template>

<script setup>
import TheSearchBar from '@/components/TheSearchBar.vue'
import TheProductsGridVue from '@/components/TheProductsGrid.vue'
import { provide, ref, reactive, watch, computed, onMounted, inject } from 'vue'
import axios from 'axios'
const { cartItems, cartOpen } = inject('cart')

const { products, getProducts } = inject('products')

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

watch(filters, async () => {
  let params = {
    sortBy: filters.sortBy
  }
  if (filters.searchQuery) {
    params.title = `*${filters.searchQuery}*`
  }
  getProducts(params)
})
</script>

<style lang="scss" scoped></style>
