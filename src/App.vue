<template>
  <div id="app" class="mt-5">
    <search-results></search-results>
    <search-bar></search-bar>
  </div>
</template>

<script>
import SearchBar from './components/SearchBar.vue'
import SearchResults from './components/SearchResults.vue'

export default {
  name: 'App',
  components: {
    SearchBar,
    SearchResults
  }
}
</script>

<style>

</style>
