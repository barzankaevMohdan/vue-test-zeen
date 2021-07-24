<template>
  <section class="player">
    <app-video></app-video>
  </section>
</template>

<script>
import AppVideo from './AppVideo'
export default {
  components: {
    AppVideo
  }
}
</script>

<style lang="scss">
  .player {
    min-height: 1500px;
    padding: 60px 0;
    background: #edeef0;
  }
</style>
