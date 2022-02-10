<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
// import Typer from './components/Typer.vue';
import TyperGenerator from './components/TyperGenerator.vue';
</script>

<template>
  <div class="bg-gray-800 min-h-screen h-auto min-w-full">
  <TyperGenerator :random_words="true"/>

  <!--  :custom_word_list="['whats','up','my','dudes','it\'s','wednesday']" -->
  </div>
  <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->

  <!-- <h2 class="text-3xl">Hello!</h2> -->
  <!-- <HelloWorld msg="Hello Vue 3 + Vite" /> -->
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center;
  color: #2c3e50; */
  /* margin-top: 60px; */
}
</style>
