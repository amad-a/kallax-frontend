<template>
<div class="app-wrapper">
  <div class="header">
      <div button="back-button" @click="back()">←</div>
      <div class="title">{{ release.release }}</div>
  </div>

  <div class="list-container">
      <img class="cover" v-if="release.image" :src="release.image">
      <div class="page-wrapper">
          <div class="artist-container">
              <div class="info-title">artist:</div>
              <div class="info-content">{{ release.artist }}</div>
          </div>
          <div class="date-container">
            <div class="info-title">released:</div>
            <div class="info-content">{{ release.year }}</div>
          </div>
          <div class="label-container">
              <div class="info-title">label:</div>
              <div class="info-content">{{ release.label }}</div>
          </div>
          <div class="genre-container">
              <div class="info-title">genre:</div>
              <div class="info-content">{{ release.genre }}</div>
          </div>
      </div>
      &nbsp;
      <div class="tracklist-label">tracklist:</div>
      <div class="tracklist-container" v-for="track in release.tracks" :key="track">
          <div class="track-name">{{ track.position }} - {{ track.title }}</div>
          <div class="track-duration">{{ track.duration }}</div>
      </div>
      
      <p>added to library on: {{release.date_added}}</p>
        <br>
      <a class="delete-button" target="_blank" rel="noopener noreferrer" v-bind:href="url">find on youtube</a>
      <div v-show="option === 'library'" class="delete-button" @click="deleteFromLibrary()">delete from library</div>
      <div v-show="option === 'explore'" class="delete-button" @click="addToLibrary()">add to library</div>
      <br>
  </div>
</div>
</template>

<script>

//options: library view - delete button DONE
//options: collections view - delete from collection TODO
//options: explore view - add to library TODO

//explore tab - get rid of sorting buttons, add timestamps, and add releaseview (+ add to library button)

export default {
  name: 'ReleaseView',
  components: {

  },

  props: {
    release: {
        type: Object
    },
    option: {
        type: String
    }
  },

  data() {
    return {
        
     
    }
  },

  computed: {
      url(){
        return `https://www.youtube.com/results?search_query=${encodeURIComponent(this.release.artist+' '+this.release.release)}`; 
      },
  },


  methods: {
      back(){
          this.$emit('back')
          console.log(this.release.tracks)
      },
      checkOption(){
          console.log("OPTION: " + this.option)
          return this.option;
      },
      deleteFromLibrary(){
          //console.log(this.release.id)
          const id = this.release.id;
          this.$emit('delete', id)
          this.$emit('back')
      },
      addToLibrary(){
          this.$emit('add', this.release)
      }
  }
}
 
</script>

<style scoped>

p {
    font-family: 'News Cycle', sans-serif;
    font-weight: bold;
    background-color: transparent;
}

.info-title {
    font-size: 1em;
    text-align: left;
    padding-bottom: 10px;
    padding-top: 10px;
    font-family: 'Inconsolata', monospace;
    font-weight: bold;
    background-color: transparent;
}

.tracklist-label {
    border-bottom: 2px dotted black;
    text-align: left;
    padding: 5px;
    padding-left: 10px;
    margin-top: 0px;
    font-size: 20px;
    font-family: 'IBM Plex Mono', monospace;
    font-weight: bold;
    background-color: transparent;
}

.info-content {
    font-size: 1.5em;
    text-align: left;
    font-family: 'Inconsolata', monospace;
    background-color: transparent;
}

.page-wrapper {
    display: grid;
    gap: 5px;
    grid-template-columns: repeat(8, 1fr);
    padding: 12px;
    background-color: transparent;
}

.tracklist-container {
    display: grid;
    grid-template-columns: repeat(8, 1fr);
    grid-template-rows: 100%;
    padding: 8px;
    padding-left: 10px;
    padding-right: 10px;
    font-size: 20px;
    border-bottom: 2px solid black;
    font-family: 'IBM Plex Mono', monospace;
    background-color: transparent;
}

.track-name {
    grid-column-start: 1;
    grid-column-end: 7;
    text-align: left;
    font-family: 'IBM Plex Mono', monospace;
    background-color: transparent;
}

.track-duration {
    grid-column-start: 8;
    grid-column-end: 9;
    text-align: right;
    font-family: 'IBM Plex Mono', monospace;
    background-color: transparent;
}

.artist-container {
  grid-column-start: 1;
  grid-column-end: 5;  
  grid-row-start: 1;
  grid-row-end: 1;  
  background-color: transparent;
}

.date-container {
    grid-column-start: 5;
    grid-column-end: 8;  
    grid-row-start: 1;
    grid-row-end: 1;  
    background-color: transparent;
}

.label-container {
    grid-column-start: 1;
    grid-column-end: 5;
    grid-row-start: 2;
    grid-row-end: 2;
    background-color: transparent;
}

.genre-container {
    grid-column-start: 5;
    grid-column-end: 8;
    grid-row-start: 2;
    grid-row-end: 2;
    background-color: transparent;
}

.item {
  border-style: solid;
  border-color: black;
  border-width: 3px;
  width: 500px;
  background-color: transparent;
}

.list-container {
    border: 2px solid black;  
    overflow-y: scroll;
    overflow-x: hidden;
    height: 78vh;
    background-color: transparent;
}

.cover {
    width: 85vw;
    margin-top: 10px;
    background-color: transparent;
    border: 2px solid black;
}

.container {
    width: auto;
    background-color: transparent;
}

.header {
    border-width: 2px 2px 0px 2px; 
    border-style: solid;
    border-color: black;
    display: grid;
    grid-template-columns: repeat(8, 1fr);
    padding: 10px 0px 10px 0px;
    font-family: 'Inconsolata', monospace;
    font-weight: bold;
    background-color: transparent;
}

.title {
    grid-column-start: 2;
    grid-column-end: 8;
    background-color: transparent;
}

.delete-button {
    border: 2px solid black;
    text-align: center;
    font-size: 20px;
    width: auto;
    margin: 30px;
    padding: 10px;
    align-items: center;
}

.delete-button:hover {
    border: 2px solid black;
    background-color: black;
    font-size: 20px;
    color: #FFF8BB;
}

@media (min-width: 480px) {
  .cover {
  width: 450px; 
  align-content: center;
  }
}


</style>