<template>
  
  <div>
    <nav> 
      <div @click="set_page('add')">add</div>
      <div @click="set_page('library')">library</div>
      <div @click="set_page('explore')">explore</div>
      <div @click="set_page('playlists')">playlists</div>
    </nav>
    
    <add-release-view 
      v-show="current_page === 'add'" 
      :library="library" 
      @add-to-library="addToLibrary"/>
    <library-view 
      v-show="current_page === 'library'" 
      :library="library"
      @delete="deleteRelease"/>
    <playlists-view 
      v-show="current_page === 'playlists'" 
      :library="library" 
      :collections="collections"/>
 </div>
</template>

<script>
import AddReleaseView from './components/AddReleaseView.vue'
import LibraryView from './components/LibraryView.vue'
import PlaylistsView from './components/PlaylistsView.vue'

export default {
  name: 'App',
  components: {
    AddReleaseView,
    LibraryView,
    PlaylistsView,
  },

  data() {
    // eslint-disable-next-line no-unused-vars
    return {
      library: [],
      collections: [],
      current_page: "library",
      }
  },

  methods: {

    deleteRelease(id){
      this.library = this.library.filter((release) => {
        release.id !== id
      })
    },

    set_page(page){
      this.current_page = page;
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