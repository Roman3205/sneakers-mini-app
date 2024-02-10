<template>
  <div class="py-7 space-y-7">
    <h2 class="px-7 text-3xl font-bold">Мои закладки</h2>
    <p v-if="favourites.length == 0" class="px-7 text-lg">Здесь пусто</p>
    <the-products-grid :products="favourites"></the-products-grid>
  </div>
</template>

<script setup>
import axios from 'axios'
import TheProductsGrid from '@/components/TheProductsGrid.vue'
import { ref, onMounted } from 'vue'

const favourites = ref([])

const getFavourites = async () => {
  try {
    const { data } = await axios.get('https://91d2bdc954865766.mokky.dev/favourites')
    favourites.value = data.map((prod) => ({ ...prod.product, isAdded: null, isFavourite: null }))
  } catch (error) {
    console.log(error)
  }
}
onMounted(() => {
  getFavourites()
})
</script>

<style lang="scss" scoped></style>
