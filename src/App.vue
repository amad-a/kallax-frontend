<template>
  
  <div>
    <nav> 
      <div @click="set_page('add')">add</div>
      <div @click="set_page('library')">library</div>
      <div @click="set_page('explore')">explore</div>
      <div @click="set_page('collections')">collections</div>
    </nav>
    
    <add-release-view v-if="current_page === 'add'" :library="library" 
                      @add-to-library="addToLibrary"/>
   
 </div>
</template>

<script>
import AddReleaseView from './components/AddReleaseView.vue'

export default {
  name: 'App',
  components: {
    AddReleaseView,
  },

  data() {
    // eslint-disable-next-line no-unused-vars
    return {
      library: [],
      current_page: "library",
      }
  },

  methods: {

    set_page(page){
      this.current_page = page;
      console.log(this.current_page)
    },

    async addToLibrary(release){
      const link = release?.link;
      if (link){
        const release_page_request = `http://localhost:1000/scrape/?link=${link}`;
        const result = await fetch(release_page_request);
        const resultData = await result.json();
        this.library.push(resultData);
      }
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