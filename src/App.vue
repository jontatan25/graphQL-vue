<template>
  <div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div>
  <div>
    <input type="text" v-model="searchTerm" />
    <p v-if="loading">Loading...</p>
    <p v-else-if="error">Something went wrong! Please try again</p>
    <template v-else>
      <p v-if="activeBook">
        Update "{{ activeBook.title }}" rating:
        <EditRating
          :initial-rating="activeBook.rating"
          :book-id="activeBook.id"
          @closeForm="activeBook = null"
        />
      </p>
      <template v-else>
        <p v-for="book in books" :key="book.id">
          {{ book.title }} - {{ book.rating }}
          <button @click="activeBook = book">Edit rating</button>
        </p>
      </template>
    </template>
  </div>
</template>

<script>
import { ref, computed } from "vue";
import { useQuery } from "@vue/apollo-composable";
import ALL_BOOKS_QUERY from "./graphql/allBooks.query.gql";
import EditRating from "./components/EditRating.vue";
export default {
  name: "App",
  components: {
    EditRating, //add EditRating to components
  },
  setup() {
    const searchTerm = ref("");
    const activeBook = ref(null);
    const { result, loading, error } = useQuery(
      ALL_BOOKS_QUERY,
      () => ({
        search: searchTerm.value,
      }),
      () => ({
        debounce: 500,
        enabled: searchTerm.value.length > 2,
      })
    );
    const books = computed(() => {
      const allBooks = result.value?.allBooks || [];
      return allBooks.map((book) => ({
        id: book.id,
        title: book.title,
        rating: book.rating,
      }));
    });
    return { books, searchTerm, loading, error, activeBook };
  },
};
</script>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
