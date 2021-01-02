<template>
  <div class="create container mx-auto">
    <h1 class="mb-4">Create Book</h1>
    <form action="#" method="POST" @submit.prevent="addBook">
      <div class="form-group">
        <label class="font-bold mb-2" for="title">Title</label>
        <input type="text" name="title" id="title" v-model="title" />
      </div>
      <div class="form-group">
        <label class="font-bold mb-2" for="author">Author</label>
        <input type="text" name="author" id="author" v-model="author" />
      </div>
      <div class="form-group">
        <label class="font-bold mb-2" for="image">Image</label>
        <input type="text" name="image" id="image" v-model="image" />
      </div>
      <div class="form-group">
        <label class="font-bold mb-2" for="description">Description</label>
        <textarea
          name="description"
          id="description"
          cols="30"
          rows="10"
          v-model="description"
        ></textarea>
      </div>
      <div class="form-group">
        <label class="font-bold mb-2" for="link">Link</label>
        <input type="text" name="link" id="link" v-model="link" />
      </div>
      <div class="form-group">
        <label class="font-bold mb-2" for="featured">Featured</label>
        <input
          type="checkbox"
          name="featured"
          id="featured"
          v-model="featured"
        />
      </div>
      <div class="form-group">
        <label class="font-bold mb-2" for="category">Category</label>
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
    };
  },
  methods: {
    addBook() {
      this.$apollo
        .mutate({
          // Query
          mutation: upsertBook,
          // Parameters
          variables: {
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
          this.$router.push("/");
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
input[type="text"], textarea {
  padding: 10px 14px;
  border: 1px solid lightgray;
  border-radius: 5px;
}
label {
  display: block;
}
button {
  padding: 16px;
  background: #027bff;
  color: white;
  border-radius: 5px;
  font-size: 16px;
}
</style>
