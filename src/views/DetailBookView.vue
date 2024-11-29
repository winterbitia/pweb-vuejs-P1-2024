<template>
  <div class="max-w-4xl mt-10 mx-auto p-6 bg-white shadow-lg rounded-lg">
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
        <p class="text-lg text-gray-700 mt-2">by {{ book.author }}</p>
        <p class="text-gray-600 mt-2">
          Published on {{ book.publishedDate }} by {{ book.publisher }}
        </p>
        <p class="mt-4">{{ book.description }}</p>

        <!-- Tags -->
        <div class="mt-4 flex flex-wrap gap-2">
          <span
            v-for="tag in book.tags"
            :key="tag"
            class="px-3 py-1 bg-yellow-100 text-sm font-semibold text-orange-700 rounded-full"
          >
            {{ tag }}
          </span>
        </div>

        <!-- Quantity and Rating -->
        <div class="mt-4">
          <p>Available: {{ book.qty }} / {{ book.initialQty }}</p>
          <p>Average Rating: {{ book.rating.average }} ({{ book.rating.count }} ratings)</p>
        </div>

        <!-- Edit and Delete Buttons -->
        <div class="mt-6 flex gap-4">
        <button
          @click="isEditing = !isEditing"
          class="mt-6 bg-orange-700 text-white px-4 py-2 rounded-full hover:bg-yellow-500"
        >
          {{ isEditing ? "Cancel Edit" : "Edit Book" }}
        </button>
        <button
          @click="deleteBook"
          class="mt-6 bg-yellow-700 text-white px-4 py-2 rounded-full hover:bg-red-600"
        >
          Delete Book
        </button>
      </div>
    </div>
  </div>

    <!-- Edit Form -->
    <div v-if="isEditing" class="mt-10">
      <h2 class="text-xl font-semibold text-gray-800">Edit Book</h2>
      <form @submit.prevent="editBook">
        <div class="mt-4">
          <label class="block text-gray-700">Title</label>
          <input
            type="text"
            v-model="editableBook.title"
            class="mt-1 block w-full border-gray-300 rounded-lg shadow-sm"
          />
        </div>

        <div class="mt-4">
          <label class="block text-gray-700">Author</label>
          <input
            type="text"
            v-model="editableBook.author"
            class="mt-1 block w-full border-gray-300 rounded-lg shadow-sm"
          />
        </div>

        <div class="mt-4">
          <label class="block text-gray-700">Published</label>
          <input
            type="text"
            v-model="editableBook.publisher"
            class="mt-1 block w-full border-gray-300 rounded-lg shadow-sm"
          />
        </div>

        <div class="mt-4">
          <label class="block text-gray-700">Description</label>
          <textarea
            v-model="editableBook.description"
            class="mt-1 block w-full border-gray-300 rounded-lg shadow-sm"
          ></textarea>
        </div>

        <div class="mt-4">
          <label class="block text-gray-700">Quantity</label>
          <input
            type="number"
            v-model="editableBook.qty"
            class="mt-1 block w-full border-gray-300 rounded-lg shadow-sm"
          />
        </div>

        <div class="mt-6">
          <button
            type="submit"
            class="bg-red-800 text-white px-4 py-2 rounded-lg hover:bg-yellow-600"
          >
            Save Changes
          </button>
        </div>
      </form>
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
      isEditing: false, 
      editableBook: {}, 
    };
  },
  async created() {
    const bookId = this.$route.params.id;
    try {
      const response = await axios.get(`http://localhost:4000/book/${bookId}`);
      this.book = response.data;
      this.editableBook = { ...this.book }; 
    } catch (error) {
      console.error("Error fetching book details:", error);
    }
  },
  methods: {
    async deleteBook() {
      const bookId = this.$route.params.id;
      try {
        await axios.delete(`http://localhost:4000/book/${bookId}`);
        alert("Book deleted successfully!");
        this.$router.push("/");
      } catch (error) {
        console.error("Error deleting book:", error);
        alert("Failed to delete the book. Please try again.");
      }
    },
    async editBook() {
      const bookId = this.$route.params.id;
      try {
        const response = await axios.put(
          `http://localhost:4000/book/${bookId}`,
          this.editableBook
        );
        this.book = response.data; 
        this.isEditing = false;
        alert("Book updated successfully!");
      } catch (error) {
        console.error("Error updating book:", error);
        alert("Failed to update the book. Please try again.");
      }
    },
  },
};
</script>