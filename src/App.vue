<template>
  <div class="app">

    <div v-if="showToast" class="toast">
      ✅ Product toegevoegd
    </div>

    <Home v-if="screen === 'home'" @nav="screen = $event" />

    <AddProduct
      v-if="screen === 'add'"
      @add="addProduct"
      @nav="screen = $event"
    />

    <ManageProducts
      v-if="screen === 'manage'"
      :products="products"
      @delete="deleteProduct"
      @update="updateProduct"
      @nav="screen = $event"
    />
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

import Home from "./components/Home.vue";
import AddProduct from "./components/AddProduct.vue";
import ManageProducts from "./components/ManageProducts.vue";

const screen = ref("home");
const products = ref([]);
const showToast = ref(false);

function addProduct(product) {
  products.value.push(product);
  save();

  showToast.value = true;
  setTimeout(() => (showToast.value = false), 2000);
}

function deleteProduct(id) {
  products.value = products.value.filter(p => p.id !== id);
  save();
}

function updateProduct(updated) {
  const i = products.value.findIndex(p => p.id === updated.id);
  if (i !== -1) {
    products.value[i] = { ...products.value[i], ...updated };
    save();
  }
}

function save() {
  localStorage.setItem("products", JSON.stringify(products.value));
}

onMounted(() => {
  const data = localStorage.getItem("products");
  if (data) products.value = JSON.parse(data);
});
</script>