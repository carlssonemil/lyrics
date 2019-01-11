<template>
  <div id="app">
    <h1>🎶 Lyrics</h1>

    <form @submit.prevent="search(input)">
      <div>
        <label for="artist">Artist</label>
        <input type="text" id="artist" name="artist" placeholder="E.g. Post Malone" v-model="input.artist">
      </div>
      
      <div>
        <label for="song">Song</label>
        <input type="text" id="song" name="song" placeholder="E.g. Congratulations" v-model="input.song">
      </div>

      <div>
        <button type="submit"><img alt="Search" src="./assets/search.svg"></button>
      </div>
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

* {
  box-sizing: border-box;
  font-family: $font-stack;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  &:focus {
    outline: none;
  }
}

body {
  background: $background-color;
  margin: 20px;
}

#app {
  color: #2c3e50;
  margin: 50px auto 0;
  max-width: $app-max-width;
}

h1 {
  text-align: center;
  margin-bottom: 80px;
}

form {
  display: flex;
  margin: 40px 0 20px;
  width: 100%;

  > div {
    position: relative;
    width: 100%;
  }

  > div + div {
    margin-left: 20px;
  }

  > div:last-of-type {
    display: flex;
    flex: 0;
  }
}

label {
  color: $primary-color;
  display: block;
  font-weight: 500;
  left: 5px;
  position: absolute;
  top: -30px;
}

input {
  background: white;
  border: none;
  border-radius: $border-radius;
  box-shadow: 0px 2px 10px 0px darken($background-color, 5%);
  font-size: 1.2em;
  font-weight: 500;
  padding: 10px 15px;
  transition: 0.15s ease;
  width: 100%;

  &:focus {
    box-shadow: 0px 2px 15px 0px darken($background-color, 10%);
  }
}

button {
  background: $primary-color;
  border: none;
  border-radius: $border-radius;
  cursor: pointer;
  display: flex;
  padding: 0 18px;
  transition: 0.15s ease;

  &:hover {
    background: $primary-color-darker;
  }

  img {
    height: 16px;
  }
}
</style>
