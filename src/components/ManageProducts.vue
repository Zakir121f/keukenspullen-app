<template>
  <div>
    <h2 class="title">Producten</h2>

    <input
      class="search"
      v-model="search"
      placeholder="🔍 Zoek product..."
    />

    <p v-if="filtered.length === 0" class="empty">
      Nog geen producten gevonden
    </p>

    <div v-for="p in filtered" :key="p.id" class="card">

      <!-- EDIT -->
      <div v-if="editId === p.id">
        <input v-model="editName" />
        <input v-model.number="editCost" type="number" />
        <input v-model.number="editSell" type="number" />

        <button class="btn primary" @click="saveEdit(p.id)">
          Opslaan
        </button>
      </div>

      <!-- NORMAL -->
      <div v-else>
        <img v-if="p.image" :src="p.image" />

        <h3>{{ p.name }}</h3>
        <p>Inkoop: €{{ p.costPrice }}</p>
        <p>Verkoop: €{{ p.sellPrice }}</p>

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
const editCost = ref(0);
const editSell = ref(0);

function startEdit(p) {
  editId.value = p.id;
  editName.value = p.name;
  editCost.value = p.costPrice;
  editSell.value = p.sellPrice;
}

function saveEdit(id) {
  emit("update", {
    id,
    name: editName.value,
    costPrice: editCost.value,
    sellPrice: editSell.value,
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
</script>