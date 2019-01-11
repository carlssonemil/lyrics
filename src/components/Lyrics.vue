<template>
  <transition name="fade">
    <div class="lyrics">
      <p>{{ lyrics }}</p>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'Lyrics',
  props: {
    lyrics: String
  }
}
</script>

<style lang="scss" scoped>
p {
  font-size: 1.2em;
  line-height: 1.3;
  white-space: pre-line;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
