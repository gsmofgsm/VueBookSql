<template>
  <div class="book container mx-auto">
    <ApolloQuery
      :query="require('@/graphql/queries/Book.gql')"
      :variables="{ id: $route.params.id }"
    >
      <!-- The result will automatically updated -->
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <!-- Some content -->
        <div v-if="isLoading || loading">Loading...</div>
        <div v-else class="flex mt-16 flex-col lg:flex-row">
          <div>
            <img :src="data.book.image" alt="book cover" />
          </div>
          <div class="w-full lg:w-2/3 ml-0 mt-8 lg:mt-0 lg:ml-16">
          <div class="text-4xl font-bold">{{ data.book.title }}</div>
          <div class="text-2l text-gray-900 mb-8">{{ data.book.author }}</div>
          <div class="lg text-gray-900 mb-8">{{ data.book.description }}</div>
            <div class="my-12">
              <a :href="data.book.link" target="_blank" class="border border-purple-700 border-solid
                rounded text-purple-700 px-4 py-4 hover:from-purple-500 hover:text-white">View Link</a>
            </div>
          <div class="text-2l text-purple-900 mb-8">
            <router-link :to="`/books/${data.book.id}/edit`"
              >Edit</router-link
            > &middot;
            <a href="#" @click.prevent="deleteBook"
              >Delete</a
            >
          </div>
          </div>
        </div>
      </template>
    </ApolloQuery>
  </div>
</template>

<script>
import deleteBook from "@/graphql/mutations/DeleteBook.gql";

export default {
  methods: {
    deleteBook() {
      this.$apollo
        .mutate({
          mutation: deleteBook,
          variables: {
            id: this.$route.params.id,
          },
        })
        .then((data) => {
          console.log(data);
          this.$router.push("/");
        });
    },
  },
};
</script>
