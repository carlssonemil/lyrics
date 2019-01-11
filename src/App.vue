<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">

    <form @submit.prevent="search(input)">
      <label for="artist">Artist</label>
      <input type="text" id="artist" name="artist" placeholder="E.g. Post Malone" v-model="input.artist">
      
      <label for="song">Song</label>
      <input type="text" id="song" name="song" placeholder="E.g. Congratulations" v-model="input.song">
    </form>

    <Lyrics :lyrics="lyrics" />
  </div>
</template>

<script>
import Lyrics from './components/Lyrics.vue'

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
