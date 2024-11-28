<template>
  <nav class="fixed top-0 left-0 w-full bg-gradient-to-r from-navy to-cream p-4 shadow-lg z-50">
    <div class="container mx-auto flex justify-between items-center">
      <RouterLink to="/" class="text-white font-bold text-xl">P1 Book Store</RouterLink>
      <ul class="flex space-x-4">
        <li>
          <RouterLink
            to="/"
            class="font-bold text-gray-100 hover:text-blue-950 transition duration-300"
            active-class="text-white"
          >
            Home
          </RouterLink>
        </li>
        <li>
          <RouterLink
            to="/add-book"
            class="font-bold text-gray-100 hover:text-blue-950 transition duration-300"
            active-class="text-white"
          >
            Add Book
          </RouterLink>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "Navbar",
});
</script>

<style scoped>
.bg-gradient-to-r {
  background: linear-gradient(to right, rgb(1, 1, 84), #d1c7b4);
}
</style>
