<template>
  <div>
    <h2 class="title">Producten</h2>

    <input
      class="search"
      v-model="search"
      placeholder="🔍 Zoek product..."
    />

    <!-- EMPTY -->
    <p v-if="filtered.length === 0" class="empty">
      Nog geen producten gevonden
    </p>

    <div v-for="p in filtered" :key="p.id" class="card">

      <!-- EDIT -->
      <div v-if="editId === p.id">
        <input v-model="editName" />
        <input v-model.number="editPrice" type="number" />
        <input v-model.number="editMargin" type="number" />

        <button class="btn primary" @click="saveEdit(p.id)">
          Opslaan
        </button>
      </div>

      <!-- NORMAL -->
      <div v-else>
        <img v-if="p.image" :src="p.image" />

        <h3>{{ p.name }}</h3>
        <p>Inkoop: €{{ p.price }}</p>

        <p v-if="p.margin">
          Verkoop: €{{ calcPrice(p.price, p.margin) }}
        </p>

        <button class="btn secondary" @click="startEdit(p)">
          ✏️ Bewerken
        </button>

        <button class="btn" @click="confirmDelete(p.id)">
          ❌ Verwijder
        </button>
      </div>

    </div>

    <button class="btn secondary" @click="$emit('nav', 'home')">
      Terug
    </button>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const props = defineProps(["products"]);
const emit = defineEmits(["delete", "update", "nav"]);

const search = ref("");

const editId = ref(null);
const editName = ref("");
const editPrice = ref(0);
const editMargin = ref(0);

function startEdit(p) {
  editId.value = p.id;
  editName.value = p.name;
  editPrice.value = p.price;
  editMargin.value = p.margin;
}

function saveEdit(id) {
  emit("update", {
    id,
    name: editName.value,
    price: editPrice.value,
    margin: editMargin.value,
  });

  editId.value = null;
}

function confirmDelete(id) {
  if (confirm("Weet je zeker dat je wilt verwijderen?")) {
    emit("delete", id);
  }
}

const filtered = computed(() =>
  props.products.filter(p =>
    p.name.toLowerCase().includes(search.value.toLowerCase())
  )
);

function calcPrice(price, margin) {
  return (price * (1 + margin / 100)).toFixed(2);
}
</script>