<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
      <div class="bg-blue-600">Test tailwind</div>
    </div>
    <router-view />
  </div>
</template>

<style src="./assets/main.css"></style>
