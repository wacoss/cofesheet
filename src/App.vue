<template>
  <div
    class="w-full h-full fixed block top-0 left-0 bg-white opacity-75 z-50"
    v-if="loading"
  >
    <div
      class="opactiy-75 top-1/2 my-0 mx-auto block relative w-0 h-0"
      style="top: 50%"
    >
      <span class="text-2xl text-violet-900">Welcoming....</span>
    </div>
  </div>
  <div v-else>
    <div class="w-full relative mb-2.5">
      <img
        src="https://images.pexels.com/photos/683039/pexels-photo-683039.jpeg"
        class="w-full h-64 shadow-1 opacity-75 blur-sm object-cover"
      />
      <h2
        class="text-3xl text-violet-900 font-bold text-center absolute inset-0"
        style="top: 50%"
      >
        Cool API Coffee
      </h2>
    </div>
    <div class="container mx-auto px-8">
      <div class="my-2">
        <button
          class="bg-violet-500 hover:bg-violet-700 text-white font-bold py-2 px-4 rounded mr-2"
          @click="setType('')"
        >
          All
        </button>
        <button
          class="bg-transparent hover:bg-violet-500 text-blue-700 font-semibold hover:text-white py-2 px-4 border border-violet-500 hover:border-transparent rounded mr-2"
          v-for="(type, index) in allTypes"
          :key="index"
          :class="[typeActive === index ? 'bg-violet-100' : '']"
          @click="setType(type, index)"
        >
          {{ type }}
        </button>
      </div>
      <div class="grid grid-cols-2 md:grid-cols-4 gap-3">
        <div
          v-for="(item, index) in selectedItems"
          :key="index"
          class="w-full rounded shadow-md"
        >
          <img class="rounded h-64 w-full object-cover" :src="item.image" />
          <div class="px-6 py-4">
            <div class="font-bold text-xl mb-2">{{ item.name }}</div>
            <p class="text-violet-900 text-xl">${{ item.price }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
/* eslint-disable */
import { ref, onMounted, computed } from "vue";
const items = ref([]);
const type = ref("");
const typeActive = ref("0");
const loading = ref(false);

const fetchData = async () => {
  loading.value = true;
  await fetch("google sheet url here")
    .then((response) => response.json())
    .then((data) => {
      items.value = data;
      loading.value = false;
    });
};
const allTypes = computed(() => {
  const temp = items.value
    .map((item) => item.type)
    .filter((value, index, self) => self.indexOf(value) === index);
  return temp;
});

const selectedItems = computed(() => {
  let tempItems = items.value;
  if (type.value) {
    tempItems = tempItems.filter((i) => i.type === type.value);
  }

  return tempItems;
});
const setType = (e, i) => {
  type.value = e;
  typeActive.value = i;
};
onMounted(() => {
  fetchData();
});
</script>
