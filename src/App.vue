<template>
  <div>
    <div class="nav-wrapper">
      <div class="nav-button" @click="setPage('add')">add</div>
      <div class="nav-button" @click="setPage('library')">library</div>
      <div class="nav-button" @click="setPage('explore')">explore</div>
      <div class="nav-button" @click="setPage('playlists')">playlists</div>
    </div>
    
    <add-release-view
      v-show="current_page === 'add'" 
      :library="library" 
      @add-to-library="addToLibrary"/>
    <library-view 
      v-show="current_page === 'library'" 
      :library="library"
      @delete="deleteRelease"
      @sort="sortLibrary"/>
    <playlists-view 
      v-show="current_page === 'playlists'" 
      :library="library" 
      :collections="collections"
      @collection-added="addCollection"/>
    <public-view 
      v-show="current_page === 'explore'" 
      :library="library" 
      :recently_added="recently_added"/>
    
 </div>
</template>

<script>
import AddReleaseView from './components/AddReleaseView.vue'
import LibraryView from './components/LibraryView.vue'
import PlaylistsView from './components/PlaylistsView.vue'
import PublicView from './components/PublicView.vue'

export default {
  name: 'App',
  components: {
    AddReleaseView,
    LibraryView,
    PlaylistsView,
    PublicView
  },

  data() {
    // eslint-disable-next-line no-unused-vars
    return {
      library: [],
      collections: [],
      genres: [],
      current_page: "add",
      recently_added: [],
      }
  },

  methods: {

    sortLibrary(order){
      if (order === "artist"){
        this.sortArtistsAlpha();
      }
      else if (order === "release"){
        this.sortReleasesAlpha();
      }
      else if (order === "year"){
        this.sortReleasesDates()
      }
      else if (order === "genre"){
        this.sortGenre();
      }
    },

    addCollection(newCollection){
      this.collections.push(newCollection);
      localStorage.storedCollections = JSON.stringify(this.collections)
    },

    deleteRelease(id){
      let tempLibrary = this.library;
      tempLibrary = tempLibrary.filter((release) => {
        return (release.id !== id);
      })
      this.library = tempLibrary;
      localStorage.storedLibrary = JSON.stringify(this.library)
    },

    setPage(page){
      if (page === 'explore'){
        this.fetchRecent()
        console.log("recents reloaded")
      }
      this.current_page = page;
      
    },

    async fetchRecent(){
      const query = `http://localhost:1000/recent`;
      const db_request = await fetch(query)
      const parsed_recents = await db_request.json();
      console.log(parsed_recents)
      this.recently_added = parsed_recents;
    },

    async addToLibrary(release){
      const link = release?.link;
      if (link){
        const release_page_request = `http://localhost:1000/scrape/?link=${link}`;
        const result = await fetch(release_page_request);
        const resultData = await result.json();
        this.library.push(resultData);
        localStorage.storedLibrary = JSON.stringify(this.library)

      }
    },

    sortArtistsAlpha(){
        this.library.sort((a,b) => ((a.artist.toLowerCase() > b.artist.toLowerCase()) ? 1 : (a.artist.toLowerCase() < b.artist.toLowerCase()) ? -1 : 0));
        console.log(this.library);
    },
    sortReleasesAlpha(){
        this.library.sort((a,b) => ((a.release.toLowerCase() > b.release.toLowerCase()) ? 1 : (a.release.toLowerCase() < b.release.toLowerCase()) ? -1 : 0));
    },
    sortReleasesDates(){
        this.library.sort((a,b) => ((a.year > b.year) ? 1 : (a.year < b.year) ? -1 : 0));
    },
    sortGenre(){
        this.library.sort((a,b) => ((a.genre[0].toLowerCase() > b.genre[0].toLowerCase()) ? 1 : (a.genre[0].toLowerCase() < b.genre[0].toLowerCase()) ? -1 : 0));
    },
  },

  mounted() {
    if (localStorage.storedLibrary) {
      this.library = JSON.parse(localStorage.getItem('storedLibrary'));
      //console.log(test)
      console.log("lib loaded")
    }  
    if (localStorage.storedCollections) {
      this.collections = JSON.parse(localStorage.getItem('storedCollections'));
      console.log("collections loaded")
    }  
  },
  
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

.nav-wrapper {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
}

.nav-button {
  padding: 0px;
  margin: 0px;
}
  
</style>