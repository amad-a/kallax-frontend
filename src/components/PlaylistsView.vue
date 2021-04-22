<template>
  <div class="container">
    <div class="toggle-view2">
        <div class="button-inactive" @click="setPage('view')">browse lists</div>
        <div class="button-active" @click="setPage('create')">create list</div>
      </div>
    <div class="releases-container">
      <div class="release-view-pick" v-for="release in library" :key="release.title">
        <div class="artist">{{ release.artist }}</div> 
        <div class="release">{{ release.release }}</div>
        <input class="check" type="checkbox" :id="release.id" :value="release" v-model="added" @click="changeButton('create list !!')" />
      </div>
    </div>
    <div>
      <form class="submit-grid" @submit.prevent="onSubmit">
        <input id="name" placeholder="enter list name..." class="form-textbox" v-model="name">
        <input class="form-button" type="submit" :value="buttonText">
      </form>
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
      buttonText: 'create list !!'
    }
  },

  methods: {

    changeButton(label){
      this.buttonText = label;
    },

    setPage(option){
      this.$emit('toggle', option)
    },

    onSubmit(){
      if (this.name && this.added){
        let newCollection = {
          title: this.name,
          list: this.added,
        }
        this.$emit('collection-added', newCollection);
        this.name = '';
        this.added = [];
        this.buttonText = "list created.";
      }

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

.container {
  border: 2px solid black;
  height: 82.5vh;
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

.releases-container {
  overflow: scroll;
  height: 60vh;
  border-width: 0px 0px 2px 0px;
  border-style: solid;
  border-color: black
}

.toggle-view2 {
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

.list-container {
    overflow: scroll;
    height: 77vh;
    border-width: 0px 0px 2px 0px;
    border-style: solid;
    border-color: black;
}

.release-view-pick {
  display: grid;
  width: auto;
  grid-template-columns: repeat(6, 1fr);
  grid-template-rows: auto;
  border-bottom: 2px solid black;
  text-align: left;
  background-color: #FFF8BB;
  overflow: scroll;
}

.check {
  display: grid;
  grid-column-start: 6;
  grid-column-end: 8;
  grid-row-start: 1;
  grid-row-end: 3;
  height: 25px;
  place-items: center;
  margin: 25px;
}

.checkbox {
  height: 20px;
}

.release-view-pick:hover {
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

.form-label {
  grid-column-start: 1;
  grid-column-end: 2;
  grid-row-start: 1;
  grid-row-end: 1;
}

.form-textbox {
  grid-column-start: 1;
  grid-column-end: 7;
  grid-row-start: 1;
  grid-row-end: 1;
  margin: 10px 10px 0px 10px;
  padding: 8px;
  border: 2px solid black;
  font-size: 18px;
  background-color: transparent;
  font-family: 'Inconsolata', monospace;
}

.form-button {
  padding: 8px;
  margin: 0px 10px 0px 10px;
  font-size: 18px;
  grid-column-start: 1;
  grid-column-end: 7;
  grid-row-start: 2;
  grid-row-end: 2;
  background-color: transparent;
  border: 2px solid black;
  align-items: center;
  font-family: 'Inconsolata', monospace;
  font-weight: bold;
  
}

.form-button:hover {
  color: #FFF8BB;
  background-color: black;
}

.submit-grid {
  display: grid;
  height: 12vh;
  grid-template-columns: repeat(6, 1fr);
  grid-auto-rows: 1fr 1fr;
  gap: 10px;  
}

</style>
