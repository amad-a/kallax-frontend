<template>
<div>
  {{ recently_added }}
</div>
</template>
    
    

<script>



export default {
  name: 'PublicView',
  components: {
    //ReleaseView,
  },

  props: {
    library: {
      type: Array
    },
    recently_added: {
      type: Array
    }
  },
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
