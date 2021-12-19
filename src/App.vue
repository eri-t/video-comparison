<template>
  <div id="app">
    <MainFeed :videos="videos" />
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
    page0: '',
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
  beforeCreate () {

    fetch(`https://ludimos-videos-dev.s3.eu-central-1.amazonaws.com/test_jsons/feed_page_1.json`)
      .then(response => response.json())
      .then(json => {
        this.pages.push(json);
      });

  },
  mounted () {



  },
  methods: {
    isScrolledIntoView (el) {
      let rect = el.getBoundingClientRect();
      let elemTop = rect.top;
      let elemBottom = rect.bottom;

      // Only completely visible elements return true:
      let isVisible = (elemTop >= 0) && (elemBottom <= window.innerHeight);
      // Partially visible elements return true:
      // let isVisible = elemTop < window.innerHeight && elemBottom >= 0;
      // console.log(isVisible);
      return isVisible;
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
