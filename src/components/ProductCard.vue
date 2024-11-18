<template>
  <div class="product-card text-left bg-fuchsia-900 rounded-2xl overflow-hidden">
    <div class="flex justify-center bg-white">
      <img
          :src="image"
          alt="product"
          class="h-80 object-contain"
      >
    </div>
    <div class="p-5">
      <h2 class="text-2xl font-semibold tracking-tight">
        {{ title }}
      </h2>
      <p class="mt-3">
        {{ description }}
      </p>
      <p class="mt-3">
        <span class="font-semibold opacity-70">Price:</span>
        {{ price }}$
      </p>
      <p class="mt-1">
        <span class="font-semibold opacity-70">Category: </span>
        {{ category }}
      </p>
      <p class="mt-1">
        <span class="font-semibold opacity-70">Rating: </span>
        {{ rating.rate }} (Total ratings: {{ rating.count }})</p>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  title: String,
  price: Number,
  description: String,
  category: String,
  image: String,
  rating: {
    type: Object,
    default: () => ({
      rate: 0,
      count: 0,
    }),
  }
})

</script>

<style scoped>
</style>
