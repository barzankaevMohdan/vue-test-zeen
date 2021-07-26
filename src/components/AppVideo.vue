<template>
  <div>
    <div
      class="zeen-over"
      ref="video"
    >
      <slot></slot>
    </div>

    <div
      class="zeen-over zeen-over_mini"
      v-if="isActive && mini"
      @mousedown="move($event)"
      data-video="mini"
      :style="{
        bottom: indent + 'px',
        left: indent + 'px'
      }"
    >
      <div class="zeen-over__button" @click="toIcon"></div>
      <slot></slot>
    </div>

    <div
      class="zeen-over zeen-over_icon"
      v-if="icon"
      @click="toIcon"
    >
    </div>
  </div>
</template>

<script>
export default {
  name: 'ZeenOverMini',
  props: {
    isActive: {
      type: Boolean,
      default: true
    },
    drag: {
      type: Boolean,
      default: true
    },
    indent: {
      type: Number,
      default: 15
    }
  },
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
      if (this.drag) {
        const target = event.target
        const offsetX = event.offsetX
        const offsetY = event.offsetY
        const coords = target.getBoundingClientRect()
        document.onmousemove = e => {
          if (target.dataset.video === 'mini') {
            const deltaX = e.clientX - offsetX
            const deltaY = e.clientY - offsetY

            if (deltaX >= this.indent && deltaX <= (window.innerWidth - coords.width - this.indent - 65)) {
              target.style.left = deltaX + 'px'
            }
            if (deltaY >= this.indent && deltaY <= (window.innerHeight - coords.height - this.indent)) {
              target.style.top = deltaY + 'px'
            }
          }
        }
        document.onmouseup = () => {
          document.onmousemove = null
          document.onmouseup = null
        }
      }
    }
  },
  mounted () {
    const callback = (entries) => {
      entries.forEach(entry => {
        if (!entry.isIntersecting) {
          this.mini = true
        } else if (entry.isIntersecting) {
          this.mini = false
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
  .zeen-over {
    display: flex;
    width: 850px;
    height: 470px;

    background: #b3caf8;
  }

  .zeen-over_mini {
    width: 350px;
    height: 200px;
    position: fixed;

    background: #9fbaf0;
    z-index: 1000;
  }

  .zeen-over_icon {
    width: 20px;
    height: 20px;
    position: fixed;
    top: auto!important;
    bottom: 30px;
    left: 30px!important;

    border-radius: 4px;
    cursor: pointer;
  }

  .zeen-over__button {
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
