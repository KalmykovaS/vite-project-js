<template>
<!--  first case-->
<!--  parent:-->
<!--  <InputText :text="text" @set-event="text = $event" />-->
<!--  const text = ref('')-->
<!--  child:-->
<!--  <div>-->
<!--    <label>-->
<!--      <input type="text" :value="text" @input="$emit('set-event', $event.target.value)">-->
<!--    </label>-->
<!--    <button type="button" class="p-2" @click="$emit('set-event', '')">X</button>-->
<!--  </div>-->

<!--  second case until v3.4-->
<!--  parent:-->
<!--  <InputText v-model="text"/>-->
<!--  const text = ref('')-->
<!--  child:-->
<!--    <div>-->
<!--      <label>-->
<!--        <input type="text" :value="modelValue" @input="$emit('update:modelValue', $event.target.value)">-->
<!--      </label>-->
<!--      <button type="button" class="p-2" @click="$emit('update:modelValue', '')">X</button>-->
<!--    </div>-->

  <!--  third case v3.4 & same second parent case-->
      <div>
        <label>
          <input type="text" v-model="text">
        </label>
        <button type="button" class="p-2" @click="text = ''">X</button>
      </div>
</template>

<script setup>
// first case
// defineProps({
//   text: String,
// })

// second case until v3.4
// defineProps({
//   modelValue: String,
// })

// third case v3.4
const text = defineModel();
</script>

<style scoped>
</style>