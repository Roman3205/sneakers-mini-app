<template>
  <div>
    <suspense>
      <template #default>
        <div>
          <the-search-bar
            v-model:searchQuery="filters.searchQuery"
            v-model:sortBy="filters.sortBy"
          ></the-search-bar>
          <the-products-grid-vue :products="products"></the-products-grid-vue>
        </div>
      </template>
      <template #fallback>...loading</template>
    </suspense>
  </div>
</template>

<script setup>
// import TheSearchBar from '@/components/TheSearchBar.vue'
import Test from '@/components/Test.vue'
// import TheProductsGridVue from '@/components/TheProductsGrid.vue'
let TheProductsGridVue = defineAsyncComponent({
  loader: () => import('@/components/TheProductsGrid.vue'),
  suspensible: true
})
let TheSearchBar = defineAsyncComponent({
  loader: () => import('@/components/TheSearchBar.vue'),
  suspensible: true
})
import {
  provide,
  ref,
  reactive,
  watch,
  computed,
  onMounted,
  inject,
  defineAsyncComponent
} from 'vue'
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
