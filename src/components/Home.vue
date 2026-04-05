<template>
  <div>
    <h1 class="title">Keukenspullen</h1>

    <button class="btn primary" @click="$emit('nav', 'add')">
      ➕ Product toevoegen
    </button>

    <button class="btn secondary" @click="$emit('nav', 'manage')">
      📦 Product beheren
    </button>
  </div>
</template>