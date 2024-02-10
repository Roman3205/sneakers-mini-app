<template>
  <div class="px-7 py-5">
    <div class="flex lg:items-center lg:flex-row xs:flex-col xs:gap-5 justify-between">
      <h2 class="text-3xl font-bold">Все кроссовки</h2>
      <div class="flex gap-7 lg:flex-row xs:flex-col">
        <select
          @change="valueSelectDebounce"
          :value="sortBy"
          class="h-12 py-2 px-3 border rounded-md outlin-none xs:order-2"
        >
          <option value="title">По названию</option>
          <option value="price">По цене (дешевые)</option>
          <option value="-price">По цене (дорогие)</option>
        </select>
        <div class="flex relative items-center lg:order-2">
          <img class="absolute top-5 left-2" src="/search.svg" alt="" />
          <input
            @input="valueInputDebounce"
            :value="searchQuery"
            type="text"
            class="h-12 border-2 w-full rounded-md border-gray-300 transition-all duration-300 pl-7 pr-4 outline-none focus:border-gray-500"
            placeholder="Поиск..."
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, onUnmounted } from 'vue'

defineProps({
  sortBy: {
    type: String,
    default: 'title'
  },
  searchQuery: [String, Number]
})

let valueInputDebounce
let valueSelectDebounce

onMounted(() => {
  valueInputDebounce = _.debounce(valueInput, 600)
  valueSelectDebounce = _.debounce(valueSelect, 300)
})

onUnmounted(() => {
  valueInputDebounce.cancel()
  valueSelectDebounce.cancel()
})

const emit = defineEmits(['update:sort-by', 'update:search-query'])

const valueSelect = (evt) => {
  let value = evt.target.value
  emit('update:sort-by', value)
}

const valueInput = (evt) => {
  let value = evt.target.value
  emit('update:search-query', value)
}
</script>

<style lang="scss" scoped></style>
