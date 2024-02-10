<template>
  <div
    class="border-2 duration-500 xs:flex-col lg:flex-row gap-4 border-slate-200 flex items-center p-4 rounded-xl shadow-md min-w-40 sm:text-xs relative lg:p-5 xs:p-4 cursor-pointer transition-all hover:shadow-2xl"
  >
    <img class="w-20 h-20" :src="imageUrl" :alt="title" />
    <div class="flex flex-col w-full">
      <p>{{ title }}</p>
      <div class="flex justify-between items-center mt-3">
        <b>{{ price }} руб.</b>
        <img @click.prevent="emit('onClickRemove')" src="/close.svg" class="brightness-95" alt="" />
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
  isFavourite: Boolean,
  isAdded: Boolean
})

const emit = defineEmits(['onClickRemove'])
</script>

<style lang="scss" scoped></style>
