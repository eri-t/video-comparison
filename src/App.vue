<template>
  <div id="app">
    <MainFeed :videos="videos" @addPage="loadAsyncData" />
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
    pages: [],
    page: 0
  }),
  computed: {
    videos: function () {
      let videosList = []
      this.pages.forEach(page => {
        videosList.push(...page.data);
      });
      return videosList;
    }
  },

  methods: {
    loadAsyncData () {
      this.page++;
      if (this.page > 2) {
        // endpoint for page 3 does not exist
        return;
      }
      //  this.loading = true
      fetch(`https://ludimos-videos-dev.s3.eu-central-1.amazonaws.com/test_jsons/feed_page_` + this.page + `.json`)
        .then(response => response.json())
        .then(json => {
          this.pages.push(json);
          this.page = json.page;
        })
        .catch((error) => {
          // this.loading = false
          throw error
        })
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
}
</style>
