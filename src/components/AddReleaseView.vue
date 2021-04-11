<template>
  <div>
    <search-form @search-submitted="addRelease"></search-form>
    <div>
    <img v-if="releases.length !== 0" :src="releases[displayed_release].img" >
    <p v-if="releases.length !== 0">{{ releases[displayed_release].artist}}</p>
    <p v-if="releases.length !== 0">{{ releases[displayed_release].title}}</p>
    </div>
  <div>
   <button v-if="displayed_release !== 0" @click="previousRelease">prev</button>
   <button v-if="displayed_release < releases.length - 1" @click="nextRelease">next</button>
   <button v-if="releases.length !== 0" @click="addToLibrary">add</button>
  </div>
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
  },

  data() {
    // eslint-disable-next-line no-unused-vars
    return {
      releases: [],
      displayed_release: 0,
      error: false,
      }
  },


  methods: {

    addToLibrary(){
      if (this.library.some(release => release.id === this.releases[this.displayed_release].link)){
        console.log('error, release already in library')
        //error = true;
      }
      else {
      this.$emit('add-to-library', this.releases[this.displayed_release]);
      }
    },

    nextRelease(){
      if (this.displayed_release !== this.releases.length - 1){
        this.displayed_release += 1;
      }
    },

    previousRelease(){
      if (this.displayed_release > 0){
        this.displayed_release -= 1;
      }
    },

    async addRelease(release){

      const title = release.title;
      const artist = release.artist;
      const search_request = `http://localhost:1000/search/?artist=${artist}&title=${title}`;
      const result = await fetch(search_request);
      let releaseData = await result.json();
      this.releases = []
      this.displayed_release = 0;
    
      releaseData.forEach(element => {
        this.releases.push(element)
      });
    }
   
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
