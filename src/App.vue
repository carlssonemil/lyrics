<template>
  <div id="app">
    <aside :class="{ wide: !data }">
      <div>
        <h1>🎶 <span>Lyrics</span></h1>

        <form @submit.prevent="search(input)">
          <div id="search">
            <input type="text" id="query" name="query" placeholder="Search for artist or song..." v-model="input" v-on:keyup="search(input)" required>
            <div v-if="suggestions" id="results">
              <transition name="fade">
                <ul>
                  <li v-for="suggestion in suggestions" :key="suggestion.id" @click="fetchLyric(suggestion.artist.name, suggestion.title)">{{ `${suggestion.title} – ${suggestion.artist.name}` }}</li>
                </ul>
              </transition>
            </div>
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
      input: null,
      suggestions: null,
      data: null
    }
  },
  methods: {
    search(input) {
      const apiUrl  = "https://api.lyrics.ovh/suggest/";
      this.suggestions = null;

      if (input) {
        axios.get(apiUrl + input)
          .then(response => {
            this.suggestions = response.data.data;
            this.suggestions.length = 8;
          })
          .catch(error => {
            console.log(error);
          });
      }
    },
    fetchLyric(artist, title) {
      const apiUrl  = "https://api.lyrics.ovh/v1/";
      this.suggestions = this.input = null;

      if (artist && title) {
        axios.get(`${apiUrl}${artist}/${title}`)
             .then(response => {
               this.data = {
                 lyrics: response.data.lyrics,
                 artist: artist,
                 title: title
               }

               console.log(this.data);
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
  overflow: hidden;
}

#app {
  color: #2c3e50;
  display: flex;
  overflow: hidden;

  @media screen and (max-width: $mobile-break-point) {
    flex-direction: column;
  }

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
    z-index: 2;

    @media screen and (max-width: $mobile-break-point) {
      height: 100px;
      justify-content: center;
      padding: 20px;
      position: absolute;
      width: 100%;
    }

    &.wide {
      width: 100%;

      @media screen and (max-width: $mobile-break-point) {
        height: 100vh;
      }

      form {
        display: flex;
        flex-direction: column;

        @media screen and (min-width: $mobile-break-point) {
          max-width: 400px;
        }

        div {
          display: flex;
          justify-content: center;
        }
      }
    }

    > div {
      @media screen and (max-width: $mobile-break-point) {
        align-items: center;
        display: flex;

        h1 {
          margin: 0 15px 0 0;
        }

        form {
          margin: 0;
        }
      }
    }

    p {
      font-weight: 500;
      margin: 0;
      text-align: center;

      @media screen and (max-width: $mobile-break-point) {
        display: none;
      }

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
    padding: 30px 50px 50px;
    overflow-y: auto;
    width: calc(100vw - #{$aside-width});
    z-index: 1;

    @media screen and (max-width: $mobile-break-point) {
      height: calc(100vh - 100px);
      margin-top: 100px;
      width: 100%;
    }
  }
}

h1 {
  font-size: 30px;
  text-align: center;
  margin-bottom: 50px;

  @media screen and (max-width: $mobile-break-point) {
    span {
      display: none;
    }
  }
}

form {
  margin: 0 auto;
  width: 100%;
}

#search {
  position: relative;
}

#results {
  background: white;
  border-radius: $border-radius;
  box-shadow: 0px 2px 10px 0px rgba($primary-color-darker, 0.75);
  color: black;
  position: absolute;
  top: 50px;
  width: 100%;
  z-index: 99;

  @media screen and (max-width: $mobile-break-point) {
    box-shadow: 0px 2px 10px 0px rgba(0, 0 , 0, 0.2);
  }

  ul {
    list-style: none;
    margin: 0px;
    padding: 10px;
    width: 100%;

    li {
      border-radius: $border-radius;
      cursor: pointer;
      font-weight: 500;
      padding: 5px;
      transition: $transition;

      &:hover {
        background: darken(white, 10%);
      }
      
      + li {
        margin-top: 5px;
      }
    }
  }
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

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
