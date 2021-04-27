<template>
  <div>
    <div v-show="library.length === 0" class="empty-library">
      no releases added yet.
    </div>

    <div v-show="view === 'list'" class="library">
      
    <div v-if="option === 'library'" class="sort">
      <div class="label">sort by:</div> 
      <button class="sort-artist" @click="sortLibrary('artist')">A-Z artists</button>
      <button class="sort-release" @click="sortLibrary('release')">A-Z releases</button>
      <button class="sort-year" @click="sortLibrary('year')">Year</button>
      <button class="sort-genre" @click="sortLibrary('genre')">Genre</button>
      <button class="sort-date-added" @click="sortLibrary('date_added')">date added</button>
    </div>

    <div v-else-if="option === 'explore'" class="explore-title">
      browse releases added recently by other users. add them to your own library!
      </div>

    

    
      <div class="list-container">
        <div class="release-view" v-for="release in library" 
            :key="release.id" 
            @click="setRelease(release)">
          <div class="artist">{{ release.artist }}</div> 
          <div class="release">{{ release.release }}</div>
          <div v-if="option === 'explore'" class="date-added">added {{ timestamp(release.date_added) }}</div>
        </div>
      </div> 
    </div>
    
    <div v-show="view === 'release'">
      <release-view :release="displayed_release" @back="goBack()" @delete="deleteRelease" @add="addFromExplore" :option="option"></release-view>
    </div>
  </div>
</template> 

<script>

  // <button @click="deleteRelease(release.id)">X</button>

import ReleaseView from './ReleaseView.vue'
//import TimeAgo from 'javascript-time-ago'
//import en from 'javascript-time-ago/locale/en'

export default {
  name: 'LibraryView',
  components: {
    ReleaseView,
  },

  props: {
    library: {
        type: Object
    },
    option: {
        type: String
    }
  },

  data() {
    return {
      displayed_release: '',
      view: "list",
      message: '',

    }
  },


  methods: {

    timestamp(date) {
      //TimeAgo.addDefaultLocale(en);
      //const timeAgo = new TimeAgo('en-US')
      let elapsed = (Date.now() - Date.parse(date)) / (1000*60*60*24);
      let elapsed_days = Math.floor(elapsed)
      let elapsed_hours = Math.floor((Date.now() - Date.parse(date)) / (1000*60*60));
      //console.log(Math.floor(elapsed))

      if (elapsed < 1){
        return elapsed_hours + ' hours ago'
      } 
      else if (elapsed_days === 1){
        return elapsed_days + " day ago"
      }
      return elapsed_days + " days ago"
      },

    goBack(){
      this.view = "list"
    },

    addFromExplore(release){
      this.$emit('add', release)
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
      console.log(id + "deleted")
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
  display: grid;
  place-items: center;
  font-family: 'IBM Plex Mono', monospace;
  font-style: italic;
  
  text-align: center;
  grid-column-start: 1;
  grid-column-end: 2;
  grid-row-start: 1;
  grid-row-end: 1;
}

.explore-title {
  display: grid;
  place-items: center;
  text-align: center;
  padding: 15px;
  border-width: 2px 2px 0px 2px;
  border-color: black;
  border-style: solid;
  font-family: 'IBM Plex Mono', monospace;
  font-style: italic;
}

.list-container {
    border: 2px solid black;  
    overflow-y: scroll;
    overflow-x: hidden;
    height: 73vh;
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

.date-added {
  padding: 0px 10px 10px 10px;
  grid-column-start: 4;
  grid-column-end: 7;
  grid-row-start: 2;
  grid-row-end: 2;
  font-family: 'Inconsolata', monospace;
  font-weight: 400;
  text-align: right;
}



.sort {
  display: grid;
  grid-template-rows: 1fr 1fr;
  grid-template-columns: repeat(3, 1fr);
  border-width: 2px 2px 0px 2px;
  border-style: solid;
  border-color: black;  
}

.sort-artist {
  margin: 2px;
  grid-column-start: 2;
  grid-column-end: 3;
  grid-row-start: 1;
  grid-row-end: 1;
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
  grid-column-end: 4;
  grid-row-start: 1;
  grid-row-end: 1;
}
.sort-date-added {
  margin: 2px;
  grid-column-start: 1;
  grid-column-end: 1;
  grid-row-start: 2;
  grid-row-end: 2;
}

.sort-genre {
  margin: 2px;
  grid-column-start: 3;
  grid-column-end: 3;
  grid-row-start: 2;
  grid-row-end: 2;
}

.sort-genre, 
.sort-year, 
.sort-release, 
.sort-artist,
.sort-date-added {
  border: 2px solid black;
  margin: 4px;
  padding: 4px;
  background-color: transparent;
  font-family: 'IBM Plex Mono', monospace;
  font-weight: 600;
  font-size: 12px;
}

.sort-genre:hover, 
.sort-year:hover, 
.sort-release:hover, 
.sort-artist:hover,
.sort-date-added:hover {
  border: 2px solid black;
  background-color: black;
  color: white;
  cursor: pointer;
}



</style>
