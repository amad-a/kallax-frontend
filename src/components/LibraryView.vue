<template>
  <div>
    <li v-show="view === 'list'" v-for="release in library" :key="release.title">
        <h2>{{ release.artist }}</h2> 
        <h4>{{ release.release }}</h4>
        <button @click="setRelease(release)">info</button>
        <button @click="deleteRelease(release.id)">X</button>
    </li>
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
    }
  },


  methods: {

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
