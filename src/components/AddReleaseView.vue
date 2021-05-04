<template>
  <div>
    <search-form class="search" @search-submitted="addRelease"></search-form>
    <div class="outer-wrapper" v-if="releases.length !== 0">
      <img class="cover-add" v-if="releases.length !== 0" :src="releases[displayed_release].img" >
      <div class="artist-add" v-if="releases.length !== 0">{{ releases[displayed_release].artist}}</div>
      <div class="release-add" v-if="releases.length !== 0">{{ releases[displayed_release].title}}</div>
      <button class="nav" @click="previousRelease">←</button>
      <button class="nav next" @click="nextRelease">→</button>
      <button class="add" v-show="releases.length !== 0" @click="addToLibrary">add</button>
   </div>
   <div class="intro-wrapper" v-else>
     kallax is a music library web app that utilizes the Discogs API 
     to allow you to compile and view a virtual collection of music using the most 
     thorough music database out there.
     it is built on the MEVN stack (mongodb, express, vue, and node js). 
     <br> &nbsp; <br>
     the app arose as a response to frustrations from the lack of (especially 
     older techno/dance records) on spotify and other commercial streaming 
     services, and an attempt to combine the unparalleled breadth of music 
     on Discogs and YouTube into a single, clean interface. 
     <br> &nbsp; <br>
     additionally, it is an homage to simple yet functional interfaces of 
     iPods and mp3 players of yesteryear, and the practice of record collecting (kallax 
     refers to kallax shelving unit from IKEA, a popular choice to house 
     vinyl among aspiring record collectors).
     
     <img src="kallax.png" alt="kallax">
     <br> &nbsp; <br>
     <p> kallax was created by <a class="link" href="https://amad.cool">amad ansari</a>,
     and is a work in progress. if you run into any bugs or have suggestions for improvement, email
     <a class="link" href="mailto: amad@nyu.edu">amad@nyu.edu</a>.</p>

     <h2>how-to-use:</h2>
     <h3>search</h3>
     you can search for releases using the search fields above. 
     after searching, you can browse through results fetched by the discogs
     API, and add them to your library with the 'add' button.
     <h3>library</h3>
     sort your library, and click on a release to view release info, cover, tracklist,
     etc., find it on youtube, or delete it from your library. 
     <h3>lists</h3>
     create 'lists' (playlists, but for compiling whole releases rather than single tracks)
     with releases added to your library, name them, and browse lists you have created.
     <h3>explore</h3>
     browse releases that other users of kallax have added, and add them to your own library.
     <h3>delete library</h3>
     <p>kallax locally stores your library and lists in browser data so it's saved whenever you exit and revisit
     the page. press the button below to <b>permanently</b> delete your library and lists. this can't be undone!</p>
     <button class="library-delete" @click="$emit('delete-library')">{{ check() }}</button>
     <br> &nbsp; <br>
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

    check(){
      if (this.library.length === 0){
        return "library is empty."
      }
      else {
        return "delete library"
      }
    },

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
      else {
        this.displayed_release = 0;
      }
    },

    previousRelease(){
      if (this.displayed_release > 0){
        this.displayed_release -= 1;
      }
      else {
        this.displayed_release = this.releases.length - 1
      }
    },

    async addRelease(release){

      const title = release.title;
      const artist = release.artist;
      const search_request = `https://kallaxapp.herokuapp.com/search/?artist=${artist}&title=${title}`;
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

p {
  margin-top: 0px;
}

.wrapper {
  display: grid;
  column-gap: 0px;
}

.outer-wrapper {
  border: 2px solid black;
  display: grid;
  grid-template-columns: repeat(8, 1fr);
  row-gap: 0px;
  padding-bottom: 40px;
  padding-top: 40px;
  margin-top: 20px;
}

.intro-wrapper {
  text-align: left;
  border: 2px solid black;
  display: grid;
  padding: 20px;
  width: auto;
  height: 60vh;
  margin-top: 20px;
  overflow-y: scroll;
}

.library-delete {
  font-family: 'IBM Plex Mono', monospace;
  border: 2px solid black;
  background-color: transparent;
  padding: 10px;
  font-size: 16px;
  margin-top: 10px;
}

.library-delete:hover {
  cursor: pointer;
  border: 2px solid black;
  background-color: red;
  color: white;
  padding: 10px;
  font-size: 16px;
}

.cover-add {
  grid-column-start: 2;
  grid-column-end: 8;  
  grid-row-start: 2;
  grid-row-end: 7;  
  width: 100%;
  border: 2px solid black;
}

.artist-add {
  grid-column-start: 2;
  grid-column-end: 7;  
  grid-row-start: 7;
  grid-row-end: 7;
  text-align: left;  
  font-family: 'IBM Plex Mono', monospace;
  font-weight: bold;
  font-size: 20px;
  padding: 5px 0px 0px 0px;
}

.release-add {
  grid-column-start: 2;
  grid-column-end: 7;  
  grid-row-start: 8;
  grid-row-end: 8;  
  text-align: left;
  font-family: 'IBM Plex Mono', monospace;
  font-weight: 500;
  font-size: 18px;
  padding: 5px 0px 10px 0px;
}

.blank {
  grid-column-start: 1;
  grid-column-end: 1;  
  grid-row-start: 1;
  grid-row-end: 1;  
}

.nav {
  border: 2px solid black;
  color: black;
  background-color: white;
  width: 80%;
  padding: 5px;
  align-items: center;
  grid-column-start: 2;
  grid-column-end: 3;  
  grid-row-start: 9;
  grid-row-end: 10;  
  background-color: transparent;
}

.next {
  grid-column-start: 3;
  grid-column-end: 4;  
  grid-row-start: 9;
  grid-row-end: 10;  
}

.nav:hover {
  color: white;
  background-color: black;
  cursor: pointer;
}

.add {
  border: 2px solid black;
  color: black;
  background-color: white;
  grid-column-start: 7;
  grid-column-end: 8;  
  grid-row-start: 9;
  grid-row-end: 9;  
  background-color: transparent;

}

.add:hover {
  color: white;
  background-color: black;
  cursor: pointer;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}


</style>
