<template>
  <section class="section">
    <div class="container">
      <section class="hero is-primary mb-3">
        <div class="hero-body">
          <p class="title">Video List</p>
          <p class="subtitle">Primary subtitle</p>
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
                Lorem ipsum dolor sit amet, consectetur adipiscing elit.
                Phasellus nec iaculis mauris. <a>@bulmaio</a>.
                <a href="#">#css</a>
                <a href="#">#responsive</a>
                <br />
                <time datetime="2016-1-1">{{
                  formatDate(props.row.created_at)
                }}</time>
              </div>
            </div>
          </div>
        </b-table-column>
      </b-table>
    </div>
  </section>
</template>

<script>
export default {
  name: 'MainFeed',
  props: {
    videos: Array,
  },
  data () {
    return {
      columns: [
        {
          field: 'id',
          label: 'ID',
        }]
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
    }
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
</style>
