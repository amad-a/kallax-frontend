<template>
  <div>
    <div v-show="library.length === 0" class="empty-library">
      no releases added yet.
    </div>
    <div v-show="view === 'list'" class="library">
    <div class="sort">
      <div class="label">sort by:</div> 
      <button class="sort-artist" @click="sortLibrary('artist')">A-Z (artists)</button>
      <button class="sort-release" @click="sortLibrary('release')">A-Z (releases)</button>
      <button class="sort-year" @click="sortLibrary('year')">Year</button>
      <button class="sort-genre" @click="sortLibrary('genre')">Genre</button>
    </div>
      <div class="list-container">
        <div class="release-view" v-for="release in library" :key="release.id" @click="setRelease(release)">
          <div class="artist">{{ release.artist }}</div> 
          <div class="release">{{ release.release }}</div>
        </div>
      </div> 
    </div>
    
    <div v-show="view === 'release'">
      <release-view :release="displayed_release" @back="goBack()"></release-view>
    </div>
  </div>
</template> 

<script>

  // <button @click="deleteRelease(release.id)">X</button>

import ReleaseView from './ReleaseView.vue'

export default {
  name: 'LibraryView',
  components: {
    ReleaseView,
  },

  props: {
    library: {
        type: Object
    },
  },

  data() {
    return {
      displayed_release: '',
      view: "list",
      message: '',

    }
  },


  methods: {

    goBack(){
      this.view = "list"
    },


    sortLibrary(button){
      this.$emit('sort', button)
    },
    
    librarySearch(release){
        const searchterm = this.message;
        if(searchterm === ''){
          console.log("EMPTY")
          return true;
        }
        else if (release.release.toLowerCase().includes(searchterm) || release.artist.toLowerCase().includes(searchterm)) {
            console.log(`MATCH: ${searchterm} - ${release.release}`)
            return true;
        }
      
        console.log("NO MATCH")
        return false;
    },

    deleteRelease(id){
      this.$emit('delete', id);
    },

    setRelease(release){
      this.displayed_release = release;
      this.view = "release";
    },

    goToList(){
      this.displayed_release = '';
      this.view = 'list';
    }
  },

  computed: {
    filteredList() {
      return this.library.filter(release => {
        return release.release.toLowerCase().includes(this.search.toLowerCase()) || release.artist.toLowerCase().includes(this.search.toLowerCase())
      })
    }
  }

}

</script>

<style scoped>


.label {
  padding: 5px;
  text-align: center;
  grid-column-start: 2;
  grid-column-end: 4;
  grid-row-start: 1;
  grid-row-end: 1;
}

.list-container {
    border: 2px solid black;  
    overflow: scroll;
    height: 80vh;
}

.release-view {
  display: grid;
  width: auto;
  grid-template-columns: repeat(6, 1fr);
  grid-template-rows: auto;
  border-bottom: 2px solid black;
  text-align: left;
}

.release-view:hover {
  cursor: pointer;
  color: white;
  background-color: black;
}

.artist {
  padding: 0px 0px 10px 10px;
  grid-column-start: 1;
  grid-column-end: 5;
  grid-row-start: 2;
  grid-row-end: 2;
  font-family: 'Inconsolata', monospace;
  font-weight: 400;
  
}

.release {
  font-size: 1.6em;
  padding: 10px;
  grid-column-start: 1;
  grid-column-end: 5;
  grid-row-start: 1;
  grid-row-end: 1;
  font-family: 'Inconsolata', monospace;
  font-weight: 600;
}

.sort {
  display: grid;
  grid-template-rows: 1fr 1fr;
  grid-template-columns: repeat(4, 1fr);
  border-width: 2px 2px 0px 2px;
  border-style: solid;
  border-color: black;  
}



.sort-artist {
  margin: 2px;
  grid-column-start: 1;
  grid-column-end: 1;
  grid-row-start: 2;
  grid-row-end: 2;
}
.sort-year {
  margin: 2px;
  grid-column-start: 2;
  grid-column-end: 2;
  grid-row-start: 2;
  grid-row-end: 2; 
}
.sort-release {
  margin: 2px;
  grid-column-start: 3;
  grid-column-end: 3;
  grid-row-start: 2;
  grid-row-end: 2;
}
.sort-genre {
  margin: 2px;
  grid-column-start: 4;
  grid-column-end: 4;
  grid-row-start: 2;
  grid-row-end: 2;
}

.sort-genre, 
.sort-year, 
.sort-release, 
.sort-artist {
  border: 2px solid black;
  margin: 5px;
  background-color: transparent;
}

.sort-genre:hover, 
.sort-year:hover, 
.sort-release:hover, 
.sort-artist:hover {
  border: 2px solid black;
  margin: 5px;
  background-color: black;
  color: white;
}



</style>
