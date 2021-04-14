<template>
    <div>
    <h2>{{ release.artist }}</h2> 
    <h4>{{ release.release }}</h4>
    <button @click="setRelease(release)">info</button>
    <button @click="deleteRelease(release.id)">X</button>
    </div>
</template>

<script>
export default {
    props: {
        release: {
            type: Object
        }
    }

}
</script>


<style scoped>

</style>