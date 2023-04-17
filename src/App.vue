<script>
import { useQuery } from "@vue/apollo-composable";
import ALL_BOOKS_QUERY from "./graphql/allBooks.query.gql";
import { ref } from "vue";
export default {
  name: "App",
  setup() {
    const searchTerm = ref("");
    const { result, loading, error } = useQuery(ALL_BOOKS_QUERY, () => ({
      search: searchTerm.value,
    }));
    console.log(result);
    // useQuery Performs query, handlesloading and error,returns result to components
    return { result, searchTerm, loading, error };
  },
};
</script>

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
    <p v-if="loading">... Loading</p>
    <p v-else-if="error">... Oops something went wrong, please try again</p>
    <template v-else>
    <p v-for="book in result.allBooks" :key="book.id">
      {{ book.title }}
    </p>
  </template>
  </div>
</template>

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
