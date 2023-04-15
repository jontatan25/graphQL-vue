<script>
import gql from "graphql-tag";
import { useQuery } from "@vue/apollo-composable";

const ALL_BOOKS_QUERY = gql`
  query AllBooks {
    allBooks {
      id
      title
      rating
    }
  }
`;
export default {
  name: "App",
  setup() {
    const {result} = useQuery(ALL_BOOKS_QUERY);
    console.log(result);
    // useQuery Performs query, handlesloading and error,returns result to components
    return { result };
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
    <p v-for="book in result.allBooks" :key="book.id">
      {{ book.title }}
    </p>
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
