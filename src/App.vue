<template>
  <div id="app">
    <MainFeed />
  </div>
</template>

<script>
import MainFeed from './components/MainFeed.vue'

export default {
  name: 'App',
  components: {
    MainFeed
  },
  data: () => ({
    page1: {},
    page2: {},
  }),
  mounted () {

    fetch(`https://ludimos-videos-dev.s3.eu-central-1.amazonaws.com/test_jsons/feed_page_1.json`)
      .then(response => response.json())
      .then(json => {
        this.page1 = json;
      });

    fetch(`https://ludimos-videos-dev.s3.eu-central-1.amazonaws.com/test_jsons/feed_page_2.json`)
      .then(response => response.json())
      .then(json => {
        this.page2 = json;
      });
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
