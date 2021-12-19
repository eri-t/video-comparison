<template>
  <div class="modal-card" style="width: auto">
    <header class="modal-card-head">
      <p class="modal-card-title">Video Comparison</p>
    </header>
    <section class="modal-card-body">
      <div class="columns is-align-items-baseline">
        <div
          class="column is-half is-flex is-flex-direction-column is-align-items-center"
          v-for="(video, index) in selectedVideos"
          :key="video.id"
        >
          <video ref="video" :src="video.source_url"></video>
          <b-button class="mt-2" @click="changeVideoState(index)">
            {{
              videosState[index] && videosState[index].play ? "Pause" : "Play"
            }}
          </b-button>
        </div>
      </div>

      <b-button @click="changeGlobalState()">{{
        globalState.play ? "Pause both" : "Play both"
      }}</b-button>
    </section>
    <footer class="modal-card-foot">
      <b-button label="Close" @click="$parent.close()" />
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
      videosState: {},
      globalState: {
        play: false,
      }
    }
  },
  methods: {
    changeVideoState (index) {
      // Create initial state for the video if it doesn't exist
      if (!this.videosState[index]) {
        this.$set(this.videosState, index, {
          play: false
        })
      }

      let isPlaying = this.videosState[index].play;
      let video = this.$refs.video[index];

      if (!isPlaying) {
        video.play();
      } else {
        video.pause();
      }

      this.videosState[index].play = !isPlaying
    },

    changeGlobalState () {

      const isPlaying = this.globalState.play;
      this.selectedVideos.forEach((el, index) => {
        // Set state for the video according to global state
        if (!this.videosState[index]) {
          this.$set(this.videosState, index, {
            play: isPlaying
          })
        }

        let video = this.$refs.video[index];
        if (!isPlaying) {
          video.play();
        } else {
          video.pause();
        }

        this.videosState[index].play = !isPlaying

      });

      this.globalState.play = !isPlaying
    }
  }
}
</script>