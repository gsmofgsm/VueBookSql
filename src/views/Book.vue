<template>
  <div class="about">
    <ApolloQuery
      :query="require('@/graphql/queries/Book.gql')"
      :variables="{ id: $route.params.id }"
    >
      <!-- The result will automatically updated -->
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <!-- Some content -->
        <div v-if="isLoading || loading">Loading...</div>
        <div v-else>
          <div>{{ data.book.title }}</div>
          <div>{{ data.book.author }}</div>
          <img :src="data.book.image" alt="book cover" />
        </div>
      </template>
    </ApolloQuery>
  </div>
</template>
