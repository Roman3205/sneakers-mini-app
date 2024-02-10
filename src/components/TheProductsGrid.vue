<template>
  <div class="px-7 pt-3 w-full">
    <div
      class="grid lg:grid-cols-4 md:gap-5 sm:gap-3 lg:gap-8 xl:gap-14 md:grid-cols-3 sm:grid-cols-2 w-full gap-y-5"
    >
      <transition-group name="products" :css="true">
        <base-product-card-vue
          v-for="product in products"
          :key="product.id"
          :image-url="product.imageUrl"
          :title="product.title"
          :price="product.price"
          :id="product.id"
          :is-added="product.isAdded"
          :is-favourite="product.isFavourite"
          @on-click-remove="() => onClickCartAction(product)"
        ></base-product-card-vue>
      </transition-group>
    </div>
  </div>
</template>

<script setup>
import BaseProductCardVue from '@/components/BaseProductCard.vue'
import { inject } from 'vue'
const onClickCartAction = inject('onClickCartAction')

defineProps({
  products: Array
})
</script>

<style scoped>
.products-move {
  transition: transform 0.45s ease;
}

.products-enter-from,
.products-enter-active {
  transition: 0.3s ease-out;
  opacity: 0;
}

.products-enter-to {
  opacity: 1;
  transition: opacity 0.4s ease-out;
}

.products-leave-from,
.products-leave-active {
  opacity: 1;
  transition: opacity 0.3s ease-in;
}

.products-leave-to {
  opacity: 0;
  transition: opacity 0.4s ease-in;
}
</style>
