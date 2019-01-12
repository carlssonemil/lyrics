<template>
  <transition name="fade">
    <div class="lyrics">
      <h3>{{ data.title }}</h3>
      <h4>{{ data.artist }}</h4>
      <p>{{ data.lyrics }}</p>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'Lyrics',
  props: {
    data: Object
  }
}
</script>

<style lang="scss" scoped>
@import '@/styles/variables.scss';

.lyrics {
  padding-bottom: 50px;
}

h3 {
  font-size: 30px;
  margin: 0;
}

h4 {
  color: $primary-color;
  margin: 5px 0 0;
}

p {
  font-size: 18px;
  font-weight: 500;
  line-height: 1.5;
  white-space: pre-line;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
