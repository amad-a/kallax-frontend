<template>
<div class="outer-wrapper-lists"> 
<div v-if="page_view === 'list'">
      <div class="toggle-view">
        <div class="button-active" @click="setPage('view')">browse lists</div>
        <div class="button-inactive" @click="setPage('create')">create list</div>
      </div>
  <div class="container-browse" v-bind:style="{ backgroundImage: 'url(' + clouds + ')' }">
      <div class="collections-wrapper" >
        <div class="collection-box" v-for="collection in collections" :key="collection" @click="setActiveCollection(collection)">
          <div class="collection-title">{{ collection.title }}</div>
        </div>
      </div>
    </div>

    

 
  </div>

  <div class="playlist-loaded" v-show="page_view === 'active'">
      <div class="header">
        <div button="back-button" @click="goBack">←</div>
        <div class="title">{{ active_collection.title }}</div>
      </div>
      <div class="list-container">
        <div class="release-view" v-for="release in active_collection.list" :key="release.id">
          <div class="artist">{{ release.artist }}</div> 
          <div class="release">{{ release.release }}</div>
        </div>
      </div> 
    </div>
    </div>
</template>

<script>

export default {
  name: 'PlaylistsView',
  components: {
  
  },

  props: {
    library: {
        type: Array
    },
    collections: {
        type: Array
    },
  },

  data() {
    // eslint-disable-next-line no-unused-vars
    return {
      added: [],
      name: '',
      page_view: 'list',
      active_collection: [],
      clouds: 'clouds-dithered.png'
    }
  },

  methods: {

    setPage(option){
      this.$emit('toggle', option)
    },

    setActiveCollection(collection) {
      this.active_collection = collection;
      this.page_view = 'active';
    },

    goBack(){
      this.active_collection = '';
      this.page_view = 'list';
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

.container-browse {
  height: 77.5vh;
  overflow: scroll;
  border-width: 2px;
}

.outer-wrapper-lists {
  border: 2px solid black;
}

.button-inactive {
  font-weight: 300;
}

.button-inactive:hover {
  text-decoration: underline;
}

.button-active {
  font-weight: bold;
}

.collections-wrapper {
  display: grid;
  grid-template-columns: 1fr 1fr;
  place-items: center;
  margin: 20px 0px 20px 0px;
  overflow: scroll;
}

.playlist-loaded {
  background-color: #FFF8BB;

}

.collection-box {
  display: grid;
  width: 100px;
  height: 100px;
  border: 2px solid black;
  place-items: center;
  margin: 15px 0px 15px 0px;
  padding: 10px;
  overflow: hidden;
  background-color: #FFF8BB;
}

.collection-box:hover {
  background-color: black;
  color: #FFF8BB;
  cursor: pointer;
}

.collection-title {
  display: flex;
  overflow: hidden;
  font-weight: bold;
  font-family: 'Inconsolata', monospace;
  font-size: 18px;
}


.list-container {
    overflow: scroll;
    height: 77vh;
    border-width: 0px 0px 2px 0px;
    border-style: solid;
    border-color: black;
}

.release-view {
  display: grid;
  width: auto;
  grid-template-columns: repeat(6, 1fr);
  grid-template-rows: auto;
  border-bottom: 2px solid black;
  text-align: left;
  background-color: #FFF8BB;
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

.toggle-view {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr;
  padding: 10px;
  border-width: 0px 0px 2px 0px; 
  border-style: solid;
  border-color: black;
  background-color: #FFF8BB;
  font-weight: bold;
}



.header {
    border-width: 0px 0px 2px 0px; 
    border-style: solid;
    border-color: black;
    display: grid;
    grid-template-columns: repeat(8, 1fr);
    padding: 10px 0px 10px 0px;
    font-family: 'Inconsolata', monospace;
    font-weight: bold;
    background-color: transparent;
    text-overflow: ellipsis;
}

.title {
    grid-column-start: 2;
    grid-column-end: 8;
    background-color: transparent;
}

@media (min-width: 480px) {

  .collections-wrapper {
  grid-template-columns: 1fr 1fr 1fr;
  }
  
}

</style>
