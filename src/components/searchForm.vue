<template>
    <div class="wrapper">
        <form class="search-form" @submit.prevent="onSubmit">
            <div class="wrapper2">
            <input class="artist-input" id="artist" placeholder="artist" v-model="artist">
            <input class="release-input" id="title" placeholder="release" v-model="title">
            <input class="search-button" type="submit" value="🔍">
            </div>
        </form>
    </div>
</template>

<script>
export default {
    data(){
        return {
            artist: '',
            title: '',
        }
    },

    methods: {
        onSubmit() {
            if (this.artist || this.title){
            let searchData = {
                artist: this.artist,
                title: this.title
                } 
            this.$emit('search-submitted', searchData)
            console.log(searchData);
            this.artist = ''
            this.title = ''
            }
        }
    }   
}

</script>

<style scoped>

.wrapper {
    padding: 0px;
    background-color: transparent;
    margin: 0px 0px 0px 0px;
}

.wrapper2 {
    display: grid; 
    grid-template-columns: repeat(5, 1fr);
    grid-template-rows: repeat(2, 1fr);
    padding: 3px;
    column-gap: 12px;
    row-gap: 12px;
    background-color: transparent;
}

.artist-input{
  border: 2px solid black;
  padding: 5px;
  padding-left: 6px;
  grid-column-start: 1;
  grid-column-end: 5;
  grid-row-start: 1;
  grid-row-end: 2;
  background-color: transparent;
  font-family: 'IBM Plex Mono', monospace;
  font-style: italic;
  font-size: 14px;
}

.release-input{
  border: 2px solid black;
  padding: 5px;
  padding-left: 6px;
  grid-column-start: 1;
  grid-column-end: 5;
  grid-row-start: 2;
  grid-row-end: 3;
  background-color: transparent;
  font-family: 'IBM Plex Mono', monospace;
  font-style: italic;
  font-size: 14px;
}

.search-button{
  border: 2px solid black;
  background-color: transparent;
  grid-column-start: 5;
  grid-column-end: 5;
  grid-row-start: 1;
  grid-row-end: 3;
  font-size: 25px;
  background-color: transparent;
}

.search-button:hover {
    background-color: black;
    cursor: pointer;
    background-color: transparent;
}

</style>