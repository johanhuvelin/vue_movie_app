<template>
  <v-container v-if="loading">
    <div class="text-xs-center">
      <v-progress-circular
        indeterminate
        :size="150"
        :width="8"
        color="green">
      </v-progress-circular>
    </div>
  </v-container>
  <v-container v-else grid-list-xl>
    <v-layout wrap>
      <v-flex xs4
              v-for="(item, index) in wholeResponse"
              :key="index"
              mb-2>
        <v-card>
          <v-img
            :src="item.Poster"
            aspect-ratio="1"
          ></v-img>
          <v-card-title primary-title>
            <div>
              <h2>{{item.Title}}</h2>
              <div>Year: {{item.Year}}</div>
              <div>Type: {{item.Type}}</div>
              <div>IMDB-id: {{item.imdbID}}</div>
            </div>
          </v-card-title>
          <v-card-actions class="justify-center">
            <v-btn rounded
                   color="green"
                   @click="singleMovie(item.imdbID)"
            >View</v-btn>
          </v-card-actions>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import axios from 'axios'

export default {
  name: 'LatestMovie',
  data () {
    return {
      wholeResponse: [],
      loading: true
    }
  },
  mounted () {
    axios
      .get('https://www.omdbapi.com/?s=star&apikey=9b29d3e2&page=1&type=movie&Content-Type=application/json')
      .then(response => {
        this.wholeResponse = response.data.Search
        this.loading = false
      })
      .catch(error => {
        console.log(error)
      })
  },
  methods: {
    singleMovie (id) {
      this.$router.push('/movie/' + id)
    }
  }
}
</script>

<style lang="stylus" scoped>
.v-progress-circular
  margin: 1rem
</style>
