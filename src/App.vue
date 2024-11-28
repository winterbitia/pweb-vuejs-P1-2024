<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import Navbar from './components/Navbar.vue';
</script>

<template>
  <div class="min-h-screen bg-orange-100">
    <Navbar />
    <main class="container mx-auto py-16 px-4">
      <RouterView />
    </main>
  </div>
</template>