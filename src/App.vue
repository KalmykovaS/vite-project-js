<template>
  <div v-if="isLoading">
    <LoaderCircle />
  </div>
  <div
      v-else
      class="grid grid-cols-1 md:grid-cols-3 gap-5"
  >
    <ProductCard
        v-for="card in data"
        :key="card.id"
        :title="card.title"
        :price="card.price"
        :description="card.description"
        :category="card.category"
        :image="card.image"
        :rating="card.rating"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import ProductCard from './components/ProductCard.vue';
import LoaderCircle from "./components/loaders/LoaderCircle.vue";

const data = ref({});
const isLoading = ref(true);

fetch('https://fakestoreapi.com/products')
    .then((data) => data.json())
    .then((json) => {
      data.value = json;
      isLoading.value = false;
    })
    .catch((err) => {
      console.error('Error:', err);
    });

</script>

<style scoped>
</style>
