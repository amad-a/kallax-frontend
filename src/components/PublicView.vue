<template>
<div>
  <library-view 
    :library="recently_added" option="explore" @add="add"/>
</div>
</template>
    
    

<script>

import LibraryView from './LibraryView.vue'
export default {
  name: 'PublicView',
  components: {
    LibraryView,
  },

  props: {
    recently_added: {
      type: Array
    }
  },
  
  methods: {
    add(release){
      this.$emit('add', release);
      console.log('release locked and loaded')
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
