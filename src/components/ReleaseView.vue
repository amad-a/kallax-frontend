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
          {{ track.name }} - {{ track.duration }}
      </ul>
      <p>date added: {{release.date_added}}</p>
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


  methods: {
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