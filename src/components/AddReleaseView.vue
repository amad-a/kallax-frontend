<template>
  <div>
    <search-form @search-submitted="addRelease"></search-form>
    <div>
    <img v-if="releases.length !== 0" :src="releases[displayed_release].img" >
    <p v-if="releases.length !== 0">{{ releases[displayed_release].artist}}</p>
    <p v-if="releases.length !== 0">{{ releases[displayed_release].title}}</p>
    </div>
  <div>
   <button @click="previousRelease">prev</button>
   <button @click="nextRelease">next</button>
   <button @click="addToLibrary">add</button>
  </div>
  <div>{{ library }}</div>
 </div>
</template>

<script>
import SearchForm from './searchForm.vue'

export default {
  name: 'AddReleaseView',
  components: {
    SearchForm
  },

  props: {
    library: {
        type: Array
    },
    displayedRelease: {
        type: Number
    },
    releases: {
        type: Array
    },
  },


  methods: {

    addRelease(){
      this.$emit('add-release');
    },

    addToLibrary(){
      this.$emit('add-to-library');
    },

    nextRelease(){
      this.$emit('next-release');
    },

    previousRelease(){
      this.$emit('previous-release');
    },
   
  }
}
 
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
