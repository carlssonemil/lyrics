<template>
  <div id="app">
    <form @submit.prevent="search(input)">
      <label for="artist">Artist</label>
      <input type="text" id="artist" name="artist" placeholder="E.g. Post Malone" v-model="input.artist">
      
      <label for="song">Song</label>
      <input type="text" id="song" name="song" placeholder="E.g. Congratulations" v-model="input.song">

      <button type="submit">Search</button>
    </form>

    <Lyrics :lyrics="lyrics" />
  </div>
</template>

<script>
import Lyrics from './components/Lyrics.vue'

const axios = require("axios");

export default {
  name: 'app',
  components: {
    Lyrics
  },
  data() {
    return {
      input: {
        artist: null,
        song: null
      },
      lyrics: null
    }
  },
  methods: {
    search(input) {
      const apiUrl  = "https://api.lyrics.ovh/v1/";
      const artist  = input.artist;
      const song    = input.song;

      if (artist && song) {
        axios.get(`${apiUrl}${artist}/${song}`)
             .then(response => {
               this.lyrics = response.data.lyrics;
             })
             .catch(error => {
               console.log(error);
             });
      }
    }
  }
}
</script>

<style lang="scss">
@import '@/styles/variables.scss';

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
