<template>
  <div class="modal-card">
    <header class="modal-card-head">
      <p class="modal-card-title">Video Comparison</p>
    </header>

    <section class="modal-card-body">
      <div class="columns is-align-items-baseline">
        <div
          class="column is-half is-flex is-flex-direction-column is-align-items-center"
          v-for="(video, index) in selectedVideos"
          :key="index"
        >
          <video
            preload
            webkit-playsinline
            playsinline
            controls
            class="compared-videos"
            ref="video"
            :src="video.source_url"
          ></video>
        </div>
      </div>

      <b-button type="is-primary" @click="changeGlobalState()"
        >{{ globalState.play ? "Pause" : "Play" }} both videos</b-button
      >
    </section>

    <footer class="modal-card-foot is-flex is-justify-content-flex-end">
      <b-button label="Close" @click="$parent.close()" class="is-primary" />
    </footer>
  </div>
</template>

<script>
export default {
  name: 'ComparisonModal',
  props: {
    selectedVideos: Array
  },
  data () {
    return {
      globalState: {
        play: false,
      }
    }
  },
  methods: {
    changeGlobalState () {
      let isPlaying = this.globalState.play;

      this.selectedVideos.forEach((el, index) => {
        let video = this.$refs.video[index];
        if (!isPlaying) {
          video.play();
        } else {
          video.pause();
        }
      });

      this.globalState.play = !isPlaying
    }
  }
}
</script>

<style>
.compared-videos {
  max-height: calc(100vh - 65px - 81px - 105px);
}
</style>