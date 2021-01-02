<template>
  <div class="home">
    <div class="hero bg-gray-300 mb-24">
      <div class="container mx-auto flex flex-col lg:flex-row lg:justify-between py-10">
        <div class="mt-10">
          <h1 class="mb-4">Book recommendation site built with GraphQL</h1>
          <p class="leading-normal mb-6">Built with Laravel (Lighthouse GraphQL), Vue.js (vue-apollo) and Tailwind CSS</p>
          <div class="flex items-center">
            <a href="#" class="bg-purple-700 text-white rounded px-4 py-4 mr-4 hover:bg-purple-500">View Books</a>
            <a href="#" class="border border-purple-700 border-solid text-purple-700 rounded px-4 py-4 hover:bg-purple-500 hover:text-white">View Screencasts</a>
          </div>
        </div>
        <div class="mt-10 lg:mt-0">
          <img src="../assets/hero.svg" alt="hero">
        </div>
      </div>
    </div> <!-- end hero -->

    <div class="container mx-auto">
      <div class="flex flex-col lg:flex-row">
        <div class="w-full lg:w-1/4 px-4 mb-12">
          <ApolloQuery :query="categoriesQuery">
            <!-- The result will automatically updated -->
            <template slot-scope="{ result: { data, loading }, isLoading }">
              <!-- Some content -->
              <div v-if="isLoading || loading">Loading...</div>
              <ul v-else class="text-lg">
                <li class="mb-6">
                <a href="#" class="text-black hover:text-gray-800" @click="selectCategory('all')">All</a>
                </li>
                <li class="mb-6">
                <a href="#" class="text-black hover:text-gray-800" @click="selectCategory('featured')"
                >Featured</a>
                </li>
                <li
                    v-for="category of data.categories"
                    :key="category.id"
                    @click="selectCategory(category.id)"
                    class="mb-6 category text-black hover:text-gray-800"
                >
                <a href="#">{{ category.name }} </a>
                </li>
                <li class="mb-6">
                  <router-link to="/books/add" class="text-black hover:text-gray-800">Add a book</router-link>
                </li>
              </ul>
            </template>
          </ApolloQuery>
        </div>
        <div class="w-full lg:w-3/4 px-4 mb-12">
          <ApolloQuery :query="query" v-if="selectedCategory === 'all'">
            <template slot-scope="{ result: { data, loading }, isLoading }">
              <div v-if="isLoading || loading">Loading...</div>
              <div v-else class="grid grid-cols-1 lg:grid-cols-3 gap-4">
                <div v-for="book of data.books" :key="book.id">
                  <book-listing :book="book"></book-listing>
                </div>
              </div>
            </template>
          </ApolloQuery>
          <ApolloQuery :query="query" :variables="{ featured: true }" v-else-if="selectedCategory === 'featured'">
            <template slot-scope="{ result: { data, loading }, isLoading }">
              <div v-if="isLoading || loading">Loading...</div>
              <div v-else class="grid grid-cols-1 lg:grid-cols-3 gap-4">
                <div v-for="book of data.booksByFeatured" :key="book.id">
                  <book-listing :book="book"></book-listing>
                </div>
              </div>
            </template>
          </ApolloQuery>
          <ApolloQuery :query="categoryQuery" :variables="{ id: selectedCategory }" v-else>
            <!-- The result will automatically updated -->
            <template slot-scope="{ result: { data, loading }, isLoading }">
              <!-- Some content -->
              <div v-if="isLoading || loading">Loading...</div>
              <div v-else class="grid grid-cols-1 lg:grid-cols-3 gap-4">
                <div v-for="book of data.category.books" :key="book.id">
                  <book-listing :book="book"></book-listing>
                </div>
              </div>
            </template>
          </ApolloQuery>
        </div>
      </div>
    </div> <!-- end container -->
  </div>
</template>

<script>
// @ is an alias to /src
import categoryQuery from "@/graphql/queries/Category.gql";
import categoriesQuery from "@/graphql/queries/Categories.gql";
import booksQuery from "@/graphql/queries/Books.gql";
import booksFeaturedQuery from "@/graphql/queries/BooksFeatured.gql";
import bookListing from "@/components/BookListing";

export default {
  name: "Home",
  components: { bookListing },
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

<style>
.link-margin {
  margin-right: 24px;
}
</style>
