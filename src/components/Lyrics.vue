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
  line-height: 1.2;
}
</style>
