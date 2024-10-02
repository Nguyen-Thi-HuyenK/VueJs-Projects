<template>
  <div id="app">
    <!-- Navbar -->
    <nav class="navbar">
      <h1 class="navbar-brand">Random Quote Generator using Vue.js</h1>
    </nav>
    
    <!-- Quote Generator -->
    <QuoteGenerator />
  </div>
</template>

<script>
import QuoteGenerator from 
'./components/QuoteGenerator.vue';

export default {
  name: 'App',
  components: {
    QuoteGenerator
  }
};
</script>

<style>
/* Navbar styles */
.navbar {
  background-color: #19A5E6;
  color: #fff;
  padding: 15px 0;
  text-align: center; 
}

.navbar-brand {
  font-size: 1.5rem;
  font-weight: bold;
}
</style>
