<template>
  <div>
    <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-60"></div>
    <div
      class="bg-white overflow-y-auto xs:text-sm o-rl lg:w-96 xs:w-52 flex flex-col h-full opacity-1 fixed right-0 top-0 z-30 lg:p-8 xs:p-5 xs:pb-8"
    >
      <my-cart-header></my-cart-header>
      <the-cart-items-list></the-cart-items-list>
      <!-- <div class="flex-1" v-if="!cartIsEmpty"></div> -->
      <my-info-cart-block
        v-if="cartIsEmpty && !showOrderCreated"
        class="flex-1"
        title="Корзина пустая"
        description="Добавьте одну пару кроссовок, чтобы сделать заказ"
        imageUrl="/package-icon.png"
      ></my-info-cart-block>
      <my-info-cart-block
        v-else-if="showOrderCreated"
        class="flex-1"
        title="Заказ оформлен!"
        :description="`Ваш заказ #${showOrderCreated} скоро будет передан курьерской доставке`"
        imageUrl="/order-success-icon.png"
      ></my-info-cart-block>
      <the-pay-elemets-cart-vue
        class="flex-1 flex flex-col justify-end"
        v-else-if="!cartIsEmpty"
      ></the-pay-elemets-cart-vue>
    </div>
  </div>
</template>

<script setup>
import MyCartHeader from '@/components/MyCartHeader.vue'
import TheCartItemsList from '@/components/TheCartItemsList.vue'
import ThePayElemetsCartVue from '@/components/ThePayElemetsCart.vue'
import MyInfoCartBlock from './MyInfoCartBlock.vue'
import { inject } from 'vue'

const { cartIsEmpty, showOrderCreated } = inject('paymentInfo')
</script>

<style scoped>
* {
  overflow-x: hidden !important;
}
</style>
