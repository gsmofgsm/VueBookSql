<template>
  <div class="home">
    <ApolloQuery :query="require('@/graphql/queries/Categories.gql')">
      <!-- The result will automatically updated -->
      <template slot-scope="{ result: { data, loading } }">
        <!-- Some content -->
        <div v-if="loading">Loading...</div>
        <ul v-else>
          <li
            v-for="category of data.categories"
            :key="category.id"
            class="category"
          >
            {{ category.id }} {{ category.name }}
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
