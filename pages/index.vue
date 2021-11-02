<template>
  <v-row>
    <v-col cols="8">
      <div id="map-wrap" style="height: 83.5vh">
        <client-only>
          <l-map :zoom=13 :center="[latitude,longitude]">
            <l-tile-layer url="http://{s}.tile.osm.org/{z}/{x}/{y}.png"></l-tile-layer>
            <l-marker :lat-lng="[latitude,longitude]"></l-marker>
          </l-map>
        </client-only>
      </div>
    </v-col>
    <v-col cols="4">
      Latitude: {{latitude}},
      Longitude: {{longitude}}
    </v-col>
  </v-row>
</template>

<script>
export default {
  data() {
    return {
      latitude: 0,
      longitude: 0,
    }
  },
  mounted() {
    this.fetchDB();
  },
  computed: {
    refLatitude() {
      return this.$fire.database.ref().ref.child('latitude');
    },
    refLongitude() {
      return this.$fire.database.ref().ref.child('longitude');
    }
  },
  watch: {
    latitude(val) {
      this.refLatitude.set(val)
    },
    longitude(val) {
      this.refLongitude.set(val)
    }
  },
  methods: {
    fetchDB() {
      this.refLatitude.on('value', (datasnapshot) => {
        this.latitude = datasnapshot.val()
      }),
      this.refLongitude.on('value', (datasnapshot) => {
        this.longitude = datasnapshot.val()
      })
    }
  }
}
</script>
