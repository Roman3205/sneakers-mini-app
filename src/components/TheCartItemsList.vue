<template>
  <div class="flex flex-col gap-5 mt-5">
    <transition-group name="cart" :css="true">
      <base-cart-item
        :title="cartItem.title"
        :image-url="cartItem.imageUrl"
        :price="cartItem.price"
        @on-click-remove="() => onClickCartAction(cartItem)"
        v-for="cartItem in cartItems"
        :key="cartItem.id"
      ></base-cart-item>
    </transition-group>
  </div>
</template>

<script setup>
import { inject } from 'vue'
import BaseCartItem from './BaseCartItem.vue'

const { cartItems } = inject('cart')
const onClickCartAction = inject('onClickCartAction')
</script>

<style scoped>
.cart-move {
  transition: transform 0.45s ease;
}

.cart-enter-from,
.cart-enter-active {
  transition: 0.3s ease-out;
  opacity: 0;
}

.cart-enter-to {
  opacity: 1;
  transition: opacity 0.4s ease-out;
}

.cart-leave-from,
.cart-leave-active {
  opacity: 1;
  transform: translateX(20px);

  transition: all 0.6s ease-in;
}

.cart-leave-to {
  opacity: 0;
  transform: translateX(150px);
  transition: all 0.55s ease-in;
}
</style>
