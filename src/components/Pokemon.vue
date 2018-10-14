<template>
    <h1>Pokemon id {{id}}</h1>
</template>

<script>
    export default {
      name: "pokemon",
      data () {
        return {
          id: this.$router.currentRoute.params['id']
        }
      }
    }
</script>

<style scoped>

</style>
