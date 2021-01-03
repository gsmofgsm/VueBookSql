<template>
  <div id="app" class="font-sans antialiased text-black">
    <div id="nav" class="bg-purple-700">
      <div class="container mx-auto flex items-center justify-between py-6">
        <div>
          <a href="/"><img src="./assets/logo.svg" alt="logo"></a>
        </div>
        <div>
          <router-link to="/" class="text-white hover:text-gray-300">Home</router-link>
          <router-link to="/about" class="text-white hover:text-gray-300 pl-6">About</router-link>
          <router-link to="/login" class="text-white hover:text-gray-300 pl-6">Login</router-link>
          <a href="#" @click.prevent="logout" class="text-white hover:text-gray-300 pl-6">Logout</a>
        </div>
      </div>
    </div>
    <router-view />
  </div>
</template>

<script>
import {onLogout} from "@/vue-apollo";

export default {
  methods: {
    logout() {
      onLogout(this.$apollo.provider.defaultClient)
      this.$router.push('/')
    }
  },
}
</script>

<style src="./assets/main.css"></style>
<style>
.form-group {
  margin-bottom: 32px;
}
input[type="text"], input[type="email"], input[type="password"], textarea {
  padding: 10px 14px;
  border: 1px solid lightgray;
  border-radius: 5px;
}
label {
  display: block;
}
button {
  padding: 8px 16px;
  background: #027bff;
  color: white;
  border-radius: 5px;
  font-size: 16px;
}
</style>
