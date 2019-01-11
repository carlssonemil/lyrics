<template>
  <div id="app">
    <aside :class="{ wide: !data }">
      <div>
        <h1>🎶 Lyrics</h1>

        <form @submit.prevent="search(input)">
            <label for="artist">Artist</label>
            <input type="text" id="artist" name="artist" placeholder="E.g. Post Malone" v-model="input.artist" required>

            <label for="song">Song</label>
            <input type="text" id="song" name="song" placeholder="E.g. Congratulations" v-model="input.song" required>

            <div>
              <button type="submit">
                <img alt="Search" src="./assets/search.svg">
                <span>Search</span>
              </button>
            </div>
        </form>
      </div>

      <p>Made with 💜 by <a href="https://github.com/carlssonemil">Emil Carlsson</a></p>
    </aside>

    <main v-if="data">
      <Lyrics :data="data" />
    </main>
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
      data: null
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
               this.data = {
                 lyrics: response.data.lyrics,
                 artist: artist,
                 song: song
               }
             })
             .catch(error => {
                console.log(error);

                if (error.response.status === 404) {
                  this.data = {
                    lyrics: "An error occured. This is most likely because that the requested artist or song does not exist.\n\nTry searching again.",
                    artist: `${error.response.status} Error`,
                    song: "Artist or song does not exist"
                  }
               }
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

  &::selection {
    background: lighten($primary-color, 5%);
    color: white;
  }
}

body {
  background: $background-color;
  margin: 0;
}

#app {
  color: #2c3e50;
  display: flex;
  overflow: hidden;

  aside {
    background: $primary-color;
    border-right: 1px solid darken($primary-color, 2%);
    color: white;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    height: 100vh;
    padding: 30px;
    transition: 0.25s ease-out;
    width: $aside-width;

    &.wide {
      width: 100%;

      form {
        display: flex;
        flex-direction: column;
        max-width: 400px;

        div {
          display: flex;
          justify-content: center;
        }
      }
    }

    p {
      font-weight: 500;
      margin: 0;
      text-align: center;

      a {
        color: white;
        border-bottom: 2px solid darken($primary-color, 5%);
        padding: 0 2px 2px;
        text-decoration: none;
        transition: $transition;

        &:hover {
          border-color: $primary-color-darker;
        }
      }
    }
  }

  main {
    height: 100vh;
    padding: 30px 50px;
    overflow-y: auto;
    width: calc(100vw - #{$aside-width});
  }
}

h1 {
  font-size: 30px;
  text-align: center;
  margin-bottom: 50px;
}

form {
  margin: 0 auto;
  width: 100%;
}

label {
  color: darken($primary-color-darker, 5%);
  display: block;
  font-weight: 500;
  margin-bottom: 5px;
}

input {
  background: white;
  border: none;
  border-radius: $border-radius;
  box-shadow: 0px 2px 10px 0px rgba($primary-color-darker, 0.75);
  font-size: 16px;
  font-weight: 500;
  margin-bottom: 15px;
  padding: 12px 15px;
  transition: $transition;
  width: 100%;

  &:focus {
    box-shadow: 0px 2px 15px 0px rgba(darken($primary-color-darker, 5%), 1);
  }
}

button {
  align-items: center;
  background: $primary-color-darker;
  border: none;
  border-radius: $border-radius;
  cursor: pointer;
  display: flex;
  height: 45px;
  margin-top: 20px;
  padding: 0 18px;
  transition: $transition;

  &:hover {
    background: darken($primary-color-darker, 5%);
  }

  img {
    height: 16px;
  }

  span {
    color: white;
    font-size: 16px;
    font-weight: 500;
    margin-left: 10px;
  }
}
</style>
