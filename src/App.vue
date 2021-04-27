<template>
  <div class="app-wrapper">
    <div class="app-title">kallax</div>
    <div class="nav-wrapper">
      <div v-if="current_page === 'add'" class="nav-button-active" @click="setPage('add')">add</div>
      <div v-else class="nav-button" @click="setPage('add')">add</div>

      <div v-if="current_page === 'library'" class="nav-button-active" @click="setPage('library')">library</div>
      <div v-else class="nav-button" @click="setPage('library')">library</div>
  
      <div v-if="current_page === 'view' || current_page === 'create'" class="nav-button-active" @click="setPage('view')">lists</div>
      <div v-else class="nav-button" @click="setPage('view')">lists</div>

      <div v-if="current_page === 'explore'" class="nav-button-active" @click="setPage('explore')">explore</div>
      <div v-else class="nav-button" @click="setPage('explore')">explore</div>
    </div>
    
    <add-release-view
      v-show="current_page === 'add'" 
      :library="library" 
      @add-to-library="addToLibrary"/>
    <library-view 
      v-show="current_page === 'library'" 
      :library="library"
      @delete="deleteRelease"
      @sort="sortLibrary"
      @add="addFromExplore"
      option="library"/>
    <playlists-view 
      v-show="current_page === 'create'" 
      :library="library" 
      :collections="collections"
      @collection-added="addCollection"
      @toggle="setPage"/>
    <playlists-browse 
      v-show="current_page === 'view'" 
      :library="library" 
      :collections="collections"
      @toggle="setPage"/>
    <public-view 
      v-show="current_page === 'explore'"
      :recently_added="recently_added"
      @add="addFromExplore"
      />

    
 </div>
</template>

<script>
import AddReleaseView from './components/AddReleaseView.vue'
import LibraryView from './components/LibraryView.vue'
import PlaylistsView from './components/PlaylistsView.vue'
import PublicView from './components/PublicView.vue'
import PlaylistsBrowse from './components/PlaylistsBrowse.vue'

export default {
  name: 'App',
  components: {
    AddReleaseView,
    LibraryView,
    PlaylistsView,
    PlaylistsBrowse,
    PublicView,
  },

  data() {
    // eslint-disable-next-line no-unused-vars
    return {
      library: [],
      collections: [],
      genres: [],
      recently_added: [],
      current_page: "add",
      }
  },

  methods: {

    pageCheck(page){
      if (page === this.current_page){
        return true;
      }
      return false;
    },

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
      else if (order === "date_added"){
        this.sortDateAdded();
      }
    },

    addCollection(newCollection){
      this.collections.push(newCollection);
      localStorage.storedCollections = JSON.stringify(this.collections)
    },

    deleteRelease(id){
      console.log(id)
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
      const query = `http://localhost:9999/recent`;
      const db_request = await fetch(query)
      const parsed_recents = await db_request.json();
      console.log(parsed_recents)
      this.recently_added = parsed_recents;
    },

    addFromExplore(checkRelease) {

      if (this.library.some(release => release.id === checkRelease.id)){
        console.log('error, release already in library')
        //error = true;
      }
      else {
        checkRelease.date_added = Date()
        this.library.push(checkRelease);
        localStorage.storedLibrary = JSON.stringify(this.library)
      }
    },

    async addToLibrary(release){
      const link = release?.link;
      if (link){
        const release_page_request = `http://localhost:9999/scrape/?link=${link}&label=${release.label}`;
        const result = await fetch(release_page_request);
        const resultData = await result.json();
        console.log("DATA: "+resultData)
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
    sortDateAdded(){
        this.library.sort((a,b) => (-(new Date(a.date_added) - new Date(b.date_added))));
    },
  },

  mounted() {
    if (localStorage.storedLibrary) {
      this.library = JSON.parse(localStorage.getItem('storedLibrary'));
      //localStorage.removeItem('storedLibrary');
      //localStorage.removeItem('storedCollections');
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

:root {
  background-color: #FFF8BB;
}
#app {
  font-family: 'IBM Plex Mono', monospace;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: black;
  margin: 5px;
}

.app-wrapper {
  padding: 0px;
  margin: 0px;
}

.nav-wrapper {
  padding-bottom: 5px;
  margin: 5px;
  display: grid;
  grid-template-columns: repeat(4, 1fr)
}

.app-title {
  font-family: 'IBM Plex Mono', monospace;
  font-style: italic;
  font-size: 22px;
  padding: 4px;
}

.nav-button {
  padding: 0px;
  margin: 0px;
  font-size: 20px;
  font-family: 'Inconsolata', monospace;  font-weight: 600;
  text-align: center;
}

.nav-button-active {
  padding: 0px;
  margin: 0px;
  font-size: 20px;
  font-family: 'Inconsolata', monospace;  font-weight: 800;
  text-decoration: underline;
  text-align: center;
}

.nav-button:hover {
  text-decoration: underline;
  cursor: pointer;
}

.div {
  overflow-x: hidden;
}

::-webkit-scrollbar {
  background: transparent;
  border-color: black;
  border-width: 2px;
  width: 3px;
}

/* Track */
::-webkit-scrollbar-track {
  background: transparent;
  border-color: black;
  
 
}

/* Handle */
::-webkit-scrollbar-thumb {
  background: black;
  border-color: black;
  border-width: 6px;

}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: black;
  border-color: black;
  border-width: 6px;
  
}


@media (min-width: 480px) {

  .app-wrapper {
  padding: 0px;
  margin: 0px;
  width: 480px; 
  align-content: center;
  }
  
}
  
</style>