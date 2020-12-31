<template>
  <div class="home">
    <ApolloQuery :query="require('@/graphql/queries/Categories.gql')">
      <!-- The result will automatically updated -->
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <!-- Some content -->
        <div v-if="isLoading || loading">Loading...</div>
        <div v-else>
          <a
            href="#"
            v-for="category of data.categories"
            :key="category.id"
            @click="selectCategory(category.id)"
            class="category link-margin"
          >
            {{ category.id }}. {{ category.name }}
          </a>
        </div>
      </template>
    </ApolloQuery>

    <!-- <ApolloQuery :query="require('@/graphql/queries/Books.gql')">
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading || loading">Loading...</div>
        <ul v-else>
          <li href="#" v-for="book of data.books" :key="book.id">
            {{ book.id }}. {{ book.title }}
          </li>
        </ul>
      </template>
    </ApolloQuery> -->

    <ApolloQuery
      :query="require('@/graphql/queries/Category.gql')"
      :variables="{ id: selectedCategory }"
    >
      <!-- The result will automatically updated -->
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <!-- Some content -->
        <div v-if="isLoading || loading">Loading...</div>
        <ul v-else>
          <li href="#" v-for="book of data.category.books" :key="book.id">
            {{ book.id }}. {{ book.title }}
          </li>
        </ul>
      </template>
    </ApolloQuery>
  </div>
</template>

<script>
// @ is an alias to /src
import gql from "graphql-tag";

export default {
  name: "Home",
  components: {},
  data() {
    return {
      selectedCategory: 1,
      categories: [],
    };
  },
  apollo: {
    // Simple query that will update the 'hello' vue property
    categories: gql`
      {
        categories {
          id
          name
        }
      }
    `,
  },
  methods: {
    selectCategory(category) {
      this.selectedCategory = category;
    },
  },
};
</script>

<style scoped>
.link-margin {
  margin-right: 24px;
}
</style>
