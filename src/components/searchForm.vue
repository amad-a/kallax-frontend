<template>
    <form class="search-form" @submit.prevent="onSubmit">
        <p>search discogs</p>
        <label for="artist">artist:</label>
        <input id="artist" v-model="artist">
        <label for="title" >title:</label>
        <input id="title" v-model="title">
        <input class="button" type="submit" value="Search">
    </form>
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

</style>