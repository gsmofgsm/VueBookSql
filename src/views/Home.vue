<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
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
