<template>
  <div class="max-w-4xl mx-auto p-6 bg-white shadow-lg rounded-lg">
    <div class="flex flex-col lg:flex-row">
      <!-- Cover Image -->
      <div class="flex-shrink-0">
        <img
          :src="book.coverImage || 'https://placehold.co/300x400'"
          :alt="book.title"
          class="w-64 h-96 object-cover rounded-lg"
        />
      </div>

      <!-- Book Details -->
      <div class="lg:ml-6 mt-4 lg:mt-0">
        <h1 class="text-3xl font-bold text-gray-800">{{ book.title }}</h1>
        <p class="text-lg text-gray-600 mt-2">by {{ book.author }}</p>
        <p class="text-gray-500 mt-2">
          Published on {{ book.publishedDate }} by {{ book.publisher }}
        </p>
        <p class="mt-4">{{ book.description }}</p>

        <!-- Tags -->
        <div class="mt-4 flex flex-wrap gap-2">
          <span
            v-for="tag in book.tags"
            :key="tag"
            class="px-3 py-1 bg-gray-200 text-sm text-gray-700 rounded-full"
          >
            {{ tag }}
          </span>
        </div>

        <!-- Quantity and Rating -->
        <div class="mt-4">
          <p>Available: {{ book.qty }} / {{ book.initialQty }}</p>
          <p>Average Rating: {{ book.rating.average }} ({{ book.rating.count }} ratings)</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      book: {
        title: "",
        author: "",
        publishedDate: "",
        publisher: "",
        description: "",
        coverImage: "",
        tags: [],
        initialQty: 0,
        qty: 0,
        rating: {
          average: 0,
          count: 0,
        },
      },
    };
  },
  async created() {
    const bookId = this.$route.params.id;
    try {
      const response = await axios.get(`http://localhost:4000/book/${bookId}`);
      this.book = response.data;
    } catch (error) {
      console.error("Error fetching book details:", error);
    }
  },
};
</script>
