<template>
  <div>
    <div v-show="library.length === 0" class="empty-library">
      no releases added yet.
    </div>
    <div v-show="view === 'list'" class="library">
    <div class="sort">sort by:
      <button @click="sortLibrary('artist')">A-Z (artists)</button>
      <button @click="sortLibrary('release')">A-Z (releases)</button>
      <button @click="sortLibrary('year')">Year</button>
      <button @click="sortLibrary('genre')">Genre</button>
    </div>
      <div class="list-container">
      <div v-for="release in library" :key="release.title">
        <li class="release-view" @click="setRelease(release)">
          <div class="artist">{{ release.artist }}</div> 
          <div class="release">{{ release.release }}</div>
        </li>
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

}

</script>

<style scoped>
.item {
  border-style: solid;
  border-color: black;
  border-width: 3px;
  width: 500px;
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
}

.release-view:hover {
  cursor: pointer;
  color: white;
  background-color: black;
}

.sort {
  border: 2px solid black;  
}

.artist {
  font-size: 1.6em;
  padding: 10px;
  grid-column-start: 1;
  grid-column-end: 5;
  grid-row-start: 1;
  grid-row-end: 1;
}

.release {
  padding: 0px 0px 10px 10px;
  grid-column-start: 1;
  grid-column-end: 5;
  grid-row-start: 2;
  grid-row-end: 2;
}

</style>
