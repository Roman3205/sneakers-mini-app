<template>
  <div
    class="shadow-md max-h-80 gap-3 flex flex-col justify-between min-w-40 sm:text-xs relative rounded-lg border-2 border-slate-200 lg:p-5 xs:p-4 cursor-pointer transition-all duration-300 hover:-translate-y-2 hover:shadow-2xl"
  >
    <img
      v-if="isFavourite !== null"
      @click.prevent="addToFavourites(id)"
      class="absolute top-4 left-4 hover:opacity-55 transition-all duration-500 brightness-95"
      :src="isFavourite ? '/like-2.svg' : '/like-1.svg'"
      alt="Like 1"
    />
    <img class="xl:h-44 lg:h-32 xs:h-48" :src="imageUrl" alt="Sneakers" />
    <p class="overflow-hidden whitespace-nowrap text-ellipsis">{{ title }}</p>
    <div class="flex justify-between">
      <div class="flex flex-col">
        <span class="text-slate-500">Цена:</span>
        <span>{{ price }} руб.</span>
      </div>
      <img
        v-if="isAdded !== null"
        @click.prevent="emit('onClickRemove')"
        class="hover:opacity-55 transition-all duration-500 brightness-95"
        :src="!isAdded ? '/plus.svg' : 'checked.svg'"
        alt="Plus"
      />
    </div>
  </div>
</template>

<script setup>
import { inject } from 'vue'

const props = defineProps({
  id: {
    type: Number
  },
  imageUrl: {
    type: String,
    default: '/sneakers/no-prod.png'
  },
  title: {
    type: String,
    default: 'Без названия'
  },
  price: {
    type: Number,
    default: 0
  },
  isFavourite: {
    type: Boolean,
    default: false
  },
  isAdded: {
    type: Boolean,
    default: false
  }
})

const addToFavourites = inject('addToFavourites')
const emit = defineEmits(['onClickRemove'])
</script>

<style lang="scss" scoped></style>
