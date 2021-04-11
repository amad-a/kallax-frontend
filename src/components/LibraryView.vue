<template>
  <div>
    <div v-show="library.length === 0" class="empty-library">
      no releases added yet.
    </div>
    <div v-if="view === 'list'" class="library">
      <input v-model="message" placeholder="edit me" />
      <li v-for="release in library" :key="release.title">
        <div :v-if="librarySearch(release)" class="releaseView">
        <h2>{{ release.artist }}</h2> 
        <h4>{{ release.release }}</h4>
        <button @click="setRelease(release)">info</button>
        <button @click="deleteRelease(release.id)">X</button>
        </div>
    </li>
    
    
    </div>
  
    <div v-show="view === 'release'">
      <release-view :release="displayed_release"></release-view>
      <button @click="goToList">back</button>
    </div>
  </div>
</template>

<script>

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

    librarySearch(release){
        const searchterm = this.message;
        if(searchterm === ''){
          console.log("EMPTY")
          
          return true
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
  }
}
 
</script>

<style scoped>
.item {
  border-style: solid;
  border-color: black;
  border-width: 3px;
  width: 500px;

}
</style>
