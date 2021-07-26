<template>
  <section class="player">
    <div class="container">
      <app-video :isActive="true" :drag="true" :indent="15"></app-video>
    </div>
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
