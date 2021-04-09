<template>
  <div>
    <div v-for="release in library" :key="release.title">
        <input type="checkbox" :id="release.id" :value="release" v-model="added" />
        <label :for="release.id">{{ release.release}}</label>
    </div>
    <form @submit.prevent="onSubmit">
        <label for="name">name:</label>
        <input id="name" v-model="name">
         <input class="button" type="submit" value="create collection">
    </form>
  
   <p>collections: {{ collections }}</p>
  



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
    }
  },

  methods: {
    onSubmit(){
      if (this.name && this.added){
        let newCollection = {
          title: this.name,
          list: this.added,
        }
        this.$emit('collection-added', newCollection);
        this.name = '';
        this.added = [];
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
</style>
