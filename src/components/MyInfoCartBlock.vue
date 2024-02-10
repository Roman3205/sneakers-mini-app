<template>
  <div class="flex flex-col items-center max-w-72 mx-auto text-center justify-center">
    <img height="80" width="80" alt="Info image" :src="imageUrl" />
    <h2 class="mt-4 text-2xl font-medium">{{ title }}</h2>
    <p class="text-gray-400 mt-2">{{ description }}</p>
  </div>
</template>

<script setup>
defineProps({
  title: String,
  description: String,
  imageUrl: String
})
</script>

<style lang="scss" scoped></style>
