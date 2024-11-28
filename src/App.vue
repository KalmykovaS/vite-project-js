<template>
  <div>
    <Header
        v-model="searchTitle"
        v-model:price="searchPrice"
    />
    <div class="container">

      <div v-if="isLoading">
        <LoaderCircle />
      </div>

      <div v-else>
        <div v-if="filteredData.length"
             class="py-3 grid grid-cols-1 md:grid-cols-3 gap-5"
        >
          <ProductCard
              v-for="card in filteredData"
              :key="card.id"
              :title="card.title"
              :price="card.price"
              :description="card.description"
              :category="card.category"
              :image="card.image"
              :rating="card.rating"
              @modal-open="handlerClick"
          />
        </div>
        <div v-else class=" h-64 py-3 flex items-center justify-center gap-5 text-3xl">
          Oops... nothing found :(
        </div>
      </div>

    </div>
    <FormOrder
        :class="{ 'active' : modalOpen }"
        @close-modal="modalOpen = false"
    />
  </div>
</template>

<script setup>
import {ref, computed, onBeforeMount} from 'vue';
import Header from "./components/header/Header.vue";
import ProductCard from "./components/ProductCard.vue";
import LoaderCircle from "./components/loaders/LoaderCircle.vue";
import FormOrder from "./components/forms/FormOrder.vue";
import axios from 'axios';

const data = ref([]);
const isLoading = ref(true);

const searchTitle = ref('');
const searchPrice = ref('');

// fetch('https://fakestoreapi.com/products')
//     .then((res) => res.json())
//     .then((json) => {
//       data.value = json;
//       isLoading.value = false;
//     })
//     .catch((err) => {
//       console.error('Error:', err);
//       isLoading.value = false;
//     });

async function fetchData() {
  try {
    isLoading.value = false;
    const response = await axios.get('https://fakestoreapi.com/products');
    isLoading.value = false;
    data.value = response.data;
  } catch (error) {
    console.error('Error:', error);
  }
}

onBeforeMount(() => {
  fetchData();
});

const filteredData = computed(() => {
  return data.value.filter((card) => {
    const matchesTitle = card.title.toLowerCase().includes(searchTitle.value.toLowerCase());
    const matchesPrice = searchPrice.value ? card.price <= searchPrice.value : true;
    return matchesTitle && matchesPrice;
  });
});

const modalOpen = ref(false)
function handlerClick() {
  modalOpen.value = true;
}
</script>