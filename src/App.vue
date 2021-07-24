<template>
  <div id="app">
    <the-header></the-header>
    <the-promo></the-promo>
    <the-player></the-player>
  </div>
</template>

<script>
import TheHeader from './components/TheHeader'
import ThePromo from './components/ThePromo'
import ThePlayer from './components/ThePlayer'

export default {
  name: 'App',
  components: {
    TheHeader,
    ThePromo,
    ThePlayer
  }
}
</script>

<style lang="scss">
  * {
    box-sizing: border-box;
    padding: 0;
    margin: 0;
  }

  .container {
    max-width: 100%;
    width: 1140px;
    margin: 0 auto;
    position: relative;
  }
</style>
