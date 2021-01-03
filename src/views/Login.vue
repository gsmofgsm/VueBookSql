<template>
  <div class="container mx-auto">
    <form action="" method="POST" @submit.prevent="login">
      <div class="my-4">
        <input type="email" v-model="username" name="username" id="username" placeholder="Email">
      </div>
      <div class="my-4">
        <input type="password" v-model="password" name="password" id="password" placeholder="Enter your password">
      </div>

      <div>
        <button type="submit">Login</button>
      </div>
    </form>
  </div>
</template>

<script>
import gql from 'graphql-tag';
import { onLogin } from "@/vue-apollo";

export default {
  data() {
    return {
      username: '',
      password: ''
    }
  },
  methods: {
    login() {
      // Call to the graphql mutation
      this.$apollo.mutate({
        // Query
        mutation: gql`mutation ($username: String! $password: String!) {
          login(input: {
            username: $username
            password: $password
          }) {
            access_token
          }
        }`,
        // Parameters
        variables: {
          username: this.username,
          password: this.password,
        },
      }).then((data) => {
        // Result
        console.log(data)
        onLogin(this.$apollo.provider.defaultClient, data.data.login.access_token)
        this.$router.push('/protected')
      }).catch((error) => {
        // Error
        console.error(error)
      })
    },
  }
}
</script>

<style scoped>

</style>
