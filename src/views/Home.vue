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
            class="category link-margin"
          >
            {{ category.id }}. {{ category.name }}
          </a>
        </div>
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
};
</script>

<style scoped>
.link-margin {
  margin-right: 24px;
}
</style>
