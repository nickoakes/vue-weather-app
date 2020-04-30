<template>
  <div class="container text-center mt-5">
    <div class="form-group">
      <input 
        type="text" 
        class="form-control" 
        v-model="searchTerm" 
        placeholder="Enter a location"
        v-on:keyup.enter="performSearch" />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import EventBus from '../../event-bus.js';

export default {
  name: 'SearchBar',
  data(){
    return{
      searchTerm: "",
      searchResults: ""
    }
  },
  methods:{
    performSearch(){
      axios.get(
        `https://api.openweathermap.org/data/2.5/weather?q=${this.searchTerm}&APPID=4c9a11eaf38f5c7f99bcebff268a4aae`
      )
      .then((response) => {
        this.searchResults = response;
        EventBus.$emit('searchPerformed', response);
        this.searchTerm = "";
      })
      .catch(() => {
        EventBus.$emit('searchError', "Sorry, the location you entered was not found.");
        this.searchTerm = "";
      });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
