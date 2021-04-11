<template>
  <div>
      <h2>{{ release.release }}</h2>
      <img v-if="release.image" :src="release.image" >
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
      convert(timestamp){
          let duration = "";
          let seconds = timestamp.substring(7,9)
          let minutes = timestamp.substring(4,6)
          let hours = timestamp.substring(2,3)

          if (hours !== "0"){
            duration.concat('',`${hours}:`)
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
</style>