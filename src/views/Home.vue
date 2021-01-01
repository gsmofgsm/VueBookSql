<template>
  <div class="home">
    <ApolloQuery :query="categoriesQuery">
      <!-- The result will automatically updated -->
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <!-- Some content -->
        <div v-if="isLoading || loading">Loading...</div>
        <div v-else>
          <a href="#" class="link-margin" @click="selectCategory('all')">All</a>
          <a href="#" class="link-margin" @click="selectCategory('featured')"
            >Featured</a
          >
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

    <div v-if="selectedCategory === 'all'">
      <ApolloQuery :query="query">
        <template slot-scope="{ result: { data, loading }, isLoading }">
          <div v-if="isLoading || loading">Loading...</div>
          <ul v-else>
            <li href="#" v-for="book of data.books" :key="book.id">
              {{ book.id }}. {{ book.title }}
            </li>
          </ul>
        </template>
      </ApolloQuery>
    </div>
    <div v-else-if="selectedCategory === 'featured'">
      <ApolloQuery :query="query" :variables="{ featured: true }">
        <template slot-scope="{ result: { data, loading }, isLoading }">
          <div v-if="isLoading || loading">Loading...</div>
          <ul v-else>
            <li href="#" v-for="book of data.booksByFeatured" :key="book.id">
              {{ book.id }}. {{ book.title }}
            </li>
          </ul>
        </template>
      </ApolloQuery>
    </div>
    <div v-else>
      <ApolloQuery :query="categoryQuery" :variables="{ id: selectedCategory }">
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
  </div>
</template>

<script>
// @ is an alias to /src
import categoryQuery from "@/graphql/queries/Category.gql";
import categoriesQuery from "@/graphql/queries/Categories.gql";
import booksQuery from "@/graphql/queries/Books.gql";
import booksFeaturedQuery from "@/graphql/queries/BooksFeatured.gql";

export default {
  name: "Home",
  components: {},
  data() {
    return {
      categoryQuery,
      categoriesQuery,
      booksQuery,
      selectedCategory: "all",
      query: booksQuery,
      categories: [],
    };
  },
  methods: {
    selectCategory(category) {
      if (category === "all") {
        this.query = booksQuery;
      } else if (category === "featured") {
        this.query = booksFeaturedQuery;
      } else {
        this.query = categoryQuery;
      }
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
