<template>
  <div class="create">
    <h1>Edit Book</h1>
    <form action="#" method="POST" @submit.prevent="editBook">
      <div class="form-group">
        <label for="title">Title</label>
        <input type="text" name="title" id="title" v-model="title" />
      </div>
      <div class="form-group">
        <label for="author">Author</label>
        <input type="text" name="author" id="author" v-model="author" />
      </div>
      <div class="form-group">
        <label for="image">Image</label>
        <input type="text" name="image" id="image" v-model="image" />
      </div>
      <div class="form-group">
        <label for="description">Description</label>
        <textarea
          name="description"
          id="description"
          cols="30"
          rows="10"
          v-model="description"
        ></textarea>
      </div>
      <div class="form-group">
        <label for="link">Link</label>
        <input type="text" name="link" id="link" v-model="link" />
      </div>
      <div class="form-group">
        <label for="featured">Featured</label>
        <input
          type="checkbox"
          name="featured"
          id="featured"
          v-model="featured"
        />
      </div>
      <div class="form-group">
        <label for="category">Category</label>
        <ApolloQuery :query="require('@/graphql/queries/Categories.gql')">
          <!-- The result will automatically updated -->
          <template slot-scope="{ result: { data, loading }, isLoading }">
            <!-- Some content -->
            <div v-if="isLoading || loading">Loading...</div>
            <select v-else v-model="category">
              <option
                v-for="category of data.categories"
                :key="category.id"
                :value="category.id"
              >
                {{ category.name }}
              </option>
            </select>
          </template>
        </ApolloQuery>
      </div>
      <div class="form-group">
        <button type="submit">Add book</button>
      </div>
    </form>
  </div>
</template>

<script>
import upsertBook from "@/graphql/mutations/UpsertBook.gql";
import book from "@/graphql/queries/Book.gql";

export default {
  data() {
    return {
      title: "",
      author: "",
      image: "",
      description: "",
      link: "",
      featured: "",
      category: "",
      book: null,
    };
  },
  apollo: {
    // Advanced query with parameters
    // The 'variables' method is watched by vue
    book: {
      query: book,
      // Reactive parameters
      variables() {
        // Use vue reactive properties here
        if (this.$route && this.$route.params) {
          return {
            id: this.$route.params.id,
          };
        }
      },
      // Optional result hook
      result({ data }) {
        this.title = data.book.title;
        this.author = data.book.author;
        this.image = data.book.image;
        this.description = data.book.description;
        this.link = data.book.link;
        this.featured = data.book.featured;
        this.category = data.book.category.id;
      },
    },
  },
  methods: {
    editBook() {
      this.$apollo
        .mutate({
          // Query
          mutation: upsertBook,
          // Parameters
          variables: {
            id: this.$route.params.id,
            title: this.title,
            author: this.author,
            image: this.image,
            link: this.link,
            description: this.description,
            featured: this.featured,
            category: +this.category,
          },
        })
        .then((data) => {
          // Result
          console.log(data);
          this.$router.push(`/books/${this.$route.params.id}`);
        })
        .catch((error) => {
          // Error
          console.error(error);
        });
    },
  },
};
</script>

<style scoped>
.form-group {
  margin-bottom: 32px;
}
input[type="text"] {
  padding: 10px 14px;
}
button {
  padding: 16px;
  background: #027bff;
  color: white;
  border-radius: 5px;
  font-size: 16px;
}
</style>
