<template>
  <div class="lyrics">
    <p>{{ lyrics }}</p>
  </div>
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
</style>
