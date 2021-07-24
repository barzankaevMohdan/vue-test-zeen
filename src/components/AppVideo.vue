<template>
  <div class="container">
    <div class="video" ref="video"></div>
    <div class="video video_mini" v-if="mini" @mousedown="move($event)" data-video="mini">
      <div class="video__button" @click="toIcon"></div>
    </div>
    <div class="video video_icon" v-if="icon" @click="toIcon"></div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      mini: false,
      icon: false,
      observer: null
    }
  },
  methods: {
    toIcon () {
      if (!this.icon) {
        this.icon = true
        this.mini = false
      } else {
        this.icon = false
        this.mini = true
      }
    },
    move (event) {
      const target = event.target
      let offsetX = event.offsetX
      let offsetY = event.offsetY
      document.onmousemove = e => {
        if (target.dataset.video === 'mini') {
          let deltaX = e.clientX - offsetX
          let deltaY = e.clientY - offsetY
          target.style.left = deltaX + 'px'
          target.style.top = deltaY + 'px'
        }
      }
      document.onmouseup = () => {
        let coords = target.getBoundingClientRect()
        if (coords.top <= 15) {
          target.style.top = 15 + 'px'
        }
        if (coords.left <= 15) {
          target.style.left = 15 + 'px'
        }
        if (coords.left >= (window.innerWidth - coords.width)) {
          target.style.left = window.innerWidth - coords.width - 30 + 'px'
        }
        if (coords.top >= (window.innerHeight - coords.height)) {
          target.style.top = window.innerHeight - coords.height - 15 + 'px'
        }
        document.onmousemove = null
        document.onmouseup = null
      }
    }
  },
  mounted () {
    const callback = (entries, observer) => {
      entries.forEach(entry => {
        if (!entry.isIntersecting) {
          this.mini = true
          observer.unobserve(this.$refs.video)
        }
      })
    }

    const createIntersectionObserver = () => {
      const options = {
        root: null,
        threshold: 0
      }

      this.observer = new IntersectionObserver(callback, options)
      this.observer.observe(this.$refs.video)
    }

    createIntersectionObserver()
  }
}
</script>

<style lang="scss">
  .video {
    display: flex;
    width: 850px;
    height: 470px;

    background: #b3caf8;
  }

  .video_mini {
    width: 350px;
    height: 200px;
    position: fixed;
    bottom: 15px;
    left: 15px;

    background: #9fbaf0;
    z-index: 1000;
  }

  .video_icon {
    width: 20px;
    height: 20px;
    position: fixed;
    top: auto!important;
    bottom: 30px;
    left: 30px!important;

    border-radius: 4px;
    cursor: pointer;
  }

  .video__button {
    width: 30px;
    height: 30px;
    margin-right: 30px;
    position: absolute;
    top: 0;
    right: -75px;

    border-top: 5px solid #839ccc;
    border-right: 5px solid #839ccc;
    transform: rotate(135deg);
    cursor: pointer;
  }
</style>
