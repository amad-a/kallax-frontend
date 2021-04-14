<template>
<div>
  <div class="header">
      <div button="back-button" @click="back()">←</div>
      <div class="title">{{ release.release }}</div>
  </div>
  <div class="list-container">
    
      <img class="cover" v-if="release.image" :src="release.image">
      <h3>{{ release.artist }} / {{ release.year }}</h3>
      <h4>genres: </h4>
      <ul v-for="genre in release.genre" v-bind:key="genre">
          {{ genre }}
      </ul>
      <ul v-for="track in release.tracks" v-bind:key="track">
          {{ track.name }} - {{ convert(track.duration) }}
      </ul>
      <p>date added: {{release.date_added}}</p>
      <a target="_blank" rel="noopener noreferrer" v-bind:href="url">find on youtube</a>
  </div>
</div>
</template>

<script>

export default {
  name: 'ReleaseView',
  components: {

  },

  props: {
    release: {
        type: Array
    },
  },

  data() {
    return {
        
     
    }
  },

  computed: {
      url(){
        return `https://www.youtube.com/results?search_query=${encodeURIComponent(this.release.artist+' '+this.release.release)}`; 
      }
  },


  methods: {

      back(){
          this.$emit('back')
      },

      convert(timestamp){
          let duration = "";
          let seconds = timestamp.substring(7,9)
          let minutes = timestamp.substring(4,6)
          let hours = timestamp.substring(2,3)

          if (hours !== "0"){
            duration.concat('', hours)
          }
          duration.concat('',`${minutes}:${seconds}`)
          console.log(duration)
          return duration;
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

.list-container {
    border: 2px solid black;  
    overflow: scroll;
    height: 80vh;
}

.cover {
    width: 85vw;
    padding-top: 20px;
}

.container {
    width: auto;
}

.header {
    border-width: 2px 2px 0px 2px; 
    border-style: solid;
    border-color: black;
    display: grid;
    grid-template-columns: repeat(8, 1fr);
    padding: 10px 0px 10px 0px;
}

.title {
    grid-column-start: 2;
    grid-column-end: 8;
}

.back-button {}

</style>