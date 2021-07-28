<template>
  <div class="some-block">
    <div class="container">
      <ZeenOverMini
        :isActive="true"
        :drag="true"
        :margin="15"
        :miniWidth="350"
        :aspectRatio="'16:9'"
      ></ZeenOverMini>
    </div>
  </div>
</template>

<script>
import ZeenOverMini from '../../../../src/components/ZeenOverMini/ZeenOverMini'

export default {
  components: {
    ZeenOverMini
  }
}
</script>

<style lang="scss">
  .container {
    max-width: 100%;
    width: 850px;
    margin: 20px auto;
  }
  .some-block {
    min-height: 1100px;
    padding: 50px 0;
  }
</style>
