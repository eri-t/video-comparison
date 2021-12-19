<template>
  <section class="section pt-0 px-0">
    <div class="container is-fullhd is-flex is-justify-content-end">
      <b-button
        :type="toggleCheckboxes ? 'is-warning' : 'is-success'"
        class="m-3 compare-btn"
        @click="toggleCheckboxes = !toggleCheckboxes"
        >{{
          this.toggleCheckboxes ? "Hide Checkboxes" : "Compare Videos"
        }}</b-button
      >
    </div>
    <div class="container is-fullhd">
      <section class="hero is-primary mb-3">
        <div class="hero-body">
          <p class="title">Video List</p>
        </div>
      </section>

      <b-table :data="videos" class="border-none" :mobile-cards="false">
        <b-table-column v-slot="props">
          <div class="card mb-2 mw-350">
            <header class="card-content">
              <div class="media">
                <div class="media-left">
                  <figure class="image is-48x48">
                    <img
                      :src="props.row.user.profile_pic"
                      alt="Placeholder image"
                    />
                  </figure>
                </div>
                <div class="media-content">
                  <p class="title is-4">
                    {{
                      props.row.user.firstname + " " + props.row.user.lastname
                    }}
                  </p>
                  <p class="subtitle is-6">Team: {{ props.row.team.name }}</p>
                </div>
              </div>
            </header>

            <div class="card-image">
              <figure class="image is-4by3">
                <img :src="props.row.thumbnail_url" alt="Placeholder image" />
              </figure>
            </div>

            <div class="card-content">
              <div class="content">
                <time datetime="2016-1-1">{{
                  formatDate(props.row.created_at)
                }}</time>
              </div>
            </div>

            <div v-if="toggleCheckboxes" class="card-footer">
              <b-field class="card-footer-item">
                <b-checkbox v-model="selectedVideos" :native-value="props.row"
                  >Compare</b-checkbox
                >
              </b-field>
            </div>
          </div>
        </b-table-column>
      </b-table>
      <div ref="end"></div>
    </div>

    <b-modal
      v-model="isComponentModalActive"
      has-modal-card
      full-screen
      :can-cancel="false"
    >
      <comparison-modal :selected-videos="selectedVideos"></comparison-modal>
    </b-modal>
  </section>
</template>

<script>
import ComparisonModal from './ComparisonModal.vue'

export default {
  name: 'MainFeed',
  props: {
    videos: Array
  },
  components: {
    ComparisonModal
  },
  data () {
    return {
      toggleCheckboxes: false,
      selectedVideos: [],
      isComponentModalActive: false,
    }
  },
  watch: {
    selectedVideos (newValue) {
      if (newValue.length == 2) {
        this.isComponentModalActive = true;
      }
    },
    toggleCheckboxes (newValue) {
      if (newValue === false) {
        this.selectedVideos = [];
      }
    },
    isComponentModalActive (newValue) {
      if (newValue === false) {
        this.selectedVideos = [];
      }
    }
  },
  methods: {
    formatDate (text) {
      // 11:09 PM - 1 Jan 2016
      // 2021-02-01T14:49:50.631Z
      let auxArr = text.split("T");
      let date = auxArr[0];
      date = date.split("-");
      date = date[2] + ' ' + this.getMonthLabel(date[1]) + ' ' + date[0];

      let time = auxArr[1].split(".")[0];
      return (time + ' - ' + date);
    },
    getMonthLabel (monthNumber) {
      let months = [
        {
          number: '1', text: 'Jan'
        },
        {
          number: '2', text: 'Feb'
        },
        {
          number: '3', text: 'Mar'
        },
        {
          number: '4', text: 'Apr'
        },
        {
          number: '5', text: 'May'
        },
        {
          number: '6', text: 'Jun'
        },
        {
          number: '7', text: 'Jul'
        },
        {
          number: '8', text: 'Aug'
        },
        {
          number: '9', text: 'Sep'
        },
        {
          number: '10', text: 'Oct'
        },
        {
          number: '11', text: 'Nov'
        },
        {
          number: '12', text: 'Dec'
        }
      ];
      return (months[(Number(monthNumber) - 1)].text);
    },

  },
  mounted () {
    this.observer = new IntersectionObserver(entries => {
      if (entries[0].intersectionRatio > 0) {
        this.$emit('addPage');
      }
    });

    this.observer.observe(this.$refs.end);
  }
}
</script>

<style>
.card.mw-350 {
  max-width: 350px;
  margin: auto;
}

.border-none .table td,
.border-none .table th {
  border: none;
}

.compare-btn {
  position: fixed;
  z-index: 1;
}
</style>
