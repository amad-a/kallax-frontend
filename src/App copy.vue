<template>
  <div>
    <add-release-view :library="library" 
                 :displayed-release="displayed_release"
                 :releases="releases"
                 @add-release="addRelease"
                 @add-to-library="addToLibrary"
                 @next-release="nextRelease"
                 @previous-release="previousRelease"/>
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
      releases: [],
      displayed_release: 0,
      library: [],
      }
  },

  methods: {

    async addToLibrary(){
      const link = this.releases[this.displayed_release]?.link;
      if (link){
        const release_page_request = `http://localhost:1000/scrape/?link=${link}`;
        const result = await fetch(release_page_request);
        const resultData = await result.json();
        this.library.push(resultData);
      }
    },

    nextRelease(){
      if (this.displayed_release !== this.releases.length - 1){
        this.displayed_release += 1;
      }
      console.log(this.displayed_release)
    },

    previousRelease(){
      if (this.displayed_release > 0){
        this.displayed_release -= 1;
      }
      console.log(this.displayed_release)
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
