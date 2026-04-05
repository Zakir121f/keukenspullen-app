<template>
  <div>
    <h2 class="title">Nieuw product</h2>

    <div class="form-group">
      <label>Product naam</label>
      <input v-model="name" />
    </div>

    <div class="form-group">
      <label>Inkoopprijs (€)</label>
      <input v-model.number="costPrice" type="number" />
    </div>

    <div class="form-group">
      <label>Verkoopprijs (€)</label>
      <input v-model.number="sellPrice" type="number" />
    </div>

    <div class="form-group">
      <label>Foto</label>
      <input type="file" @change="handleImage" />
    </div>

    <button class="btn primary" @click="save">Opslaan</button>
    <button class="btn secondary" @click="$emit('nav', 'home')">
      Terug
    </button>
  </div>
</template>

<script setup>
import { ref } from "vue";

const emit = defineEmits(["add", "nav"]);

const name = ref("");
const costPrice = ref(0);
const sellPrice = ref(0);
const image = ref("");

function handleImage(e) {
  const file = e.target.files[0];
  const reader = new FileReader();
  reader.onload = () => (image.value = reader.result);
  if (file) reader.readAsDataURL(file);
}

function save() {
  if (!name.value || !costPrice.value || !sellPrice.value) return;

  emit("add", {
    id: Date.now(),
    name: name.value,
    costPrice: costPrice.value,
    sellPrice: sellPrice.value,
    image: image.value,
  });

  window.scrollTo(0, 0);
  emit("nav", "home");
}
</script>