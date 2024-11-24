<template>
  <div class="header bg-fuchsia-900 sticky top-0 py-3 px-8 rounded-md shadow-2xl z-50">
    <div class="flex flex-wrap gap-3">
      <input
          type="text"
          class="rounded-md p-2 bg-black"
          :value="modelValue"
          placeholder="Search by name..."
          @input="$emit('update:modelValue', $event.target.value)"
      />
      <input
          type="number"
          class="rounded-md p-2 bg-black"
          :value="price"
          placeholder="Enter max price"
          @input="$emit('update:price', $event.target.value)"
      />
      <button
          type="button"
          class="p-2 bg-black"
          @click="$emit('update:modelValue', ''); $emit('update:price', '')"
      >
        Clear all
      </button>
    </div>
  </div>
</template>

<script setup>

defineProps({
  modelValue: String,
  price: String,
});

</script>

<style scoped>
</style>