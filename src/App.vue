<template>
  <div id="app">
    <dogceoApi/>
  </div>
</template>

<script>
import dogceoApi from './components/dogceoApi.vue'

export default {
  name: 'App',
  components: {
  dogceoApi: dogceoApi
  },
}
</script>

<style>
#app {
}
</style>
