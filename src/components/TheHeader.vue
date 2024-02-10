<template>
  <header
    class="flex w-full gap-3 lg:justify-between border-b-2 px-8 py-8 border-red-slate-300 flex-wrap gap-y-5 sm:justify-center"
  >
    <div @click.prevent="router.push('/')" class="cursor-pointer flex items-center gap-5">
      <img src="/logo.png" alt="Logo" />
      <div>
        <h2 class="text-xl font-bold uppercase">Best sneakers</h2>
        <p>Магазин лучших кроссовок</p>
      </div>
    </div>
    <ul class="flex items-center gap-10 flex-wrap">
      <li @click.prevent="emit('onOpenCart')" class="header__li">
        <img src="/cart.svg" alt="Cart" />
        <b>{{ totalPrice }} руб.</b>
      </li>
      <li @click.prevent="router.push({ name: 'favourites' })" class="header__li">
        <img src="/heart.svg" alt="Heart" />
        <b>Закладки</b>
      </li>
      <li class="header__li">
        <img src="/profile.svg" alt="Profile" />
        <b>Профиль</b>
      </li>
    </ul>
  </header>
</template>

<script setup>
import { inject } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const emit = defineEmits(['onOpenCart'])
const { totalPrice } = inject('paymentInfo')
</script>

<style lang="scss" scoped></style>
