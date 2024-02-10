<template>
  <div class="flex flex-col gap-4 mt-16">
    <div class="flex gap-2">
      <span>Налог:</span>
      <div class="flex-1 border-b border-b-slate-400 border-dashed"></div>
      <b>{{ tax }} руб.</b>
    </div>
    <div class="flex gap-2">
      <span>Итого:</span>
      <div class="flex-1 border-b border-b-slate-400 border-dashed"></div>
      <b>{{ totalPrice }} руб.</b>
    </div>
    <button
      :disabled="buttonCartDisabled"
      @click.prevent="createOrder"
      class="bg-lime-500 mt-2 cursor-pointer disabled:bg-slate-300 w-full rounded-xl text-center py-3 hover:bg-lime-600 transition-all duration-500 active:bg-lime-700 text-white"
    >
      Оформить заказ
    </button>
  </div>
</template>

<script setup>
import { inject } from 'vue'

const { createOrder, buttonCartDisabled } = inject('cart')
const { tax, totalPrice } = inject('paymentInfo')
</script>

<style lang="scss" scoped></style>
