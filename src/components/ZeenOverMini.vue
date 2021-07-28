<template>
  <div>
    <div
      class="zeen-over"
      ref="zeenOver"
    >
      <slot></slot>
    </div>

    <div
      class="zeen-over zeen-over_mini"
      :class="{
        'zeen-over_mini_aspect_ratio_4_3': aspectRatio === '4:3',
        'zeen-over_mini_aspect_ratio_1_1': aspectRatio === '1:1'
      }"
      :style="{
        '--zeen-over-mini-width': miniWidth + 'px',
        '--zeen-over-mini-bottom': margin + 'px',
        '--zeen-over-mini-left': margin + 'px'
      }"
      v-if="isActive && mini"
      @mousedown="move($event)"
      data-zeen-over="mini"
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
    margin: {
      type: Number,
      default: 15
    },
    miniWidth: {
      type: Number,
      default: 350
    },
    aspectRatio: {
      type: String,
      default: '16:9',
      validator: (ratio) => ['16:9', '4:3', '1:1'].includes(ratio)
    }
  },
  data () {
    return {
      mini: false,
      icon: false,
      observer: null
    }
  },
  mounted () {
    const observerOptions = {
      root: null,
      threshold: 0
    }

    const observerCall = entries => {
      entries.forEach(entry => {
        if (!entry.isIntersecting) {
          this.mini = true
          this.icon = false
        } else if (entry.isIntersecting) {
          this.mini = false
          this.icon = false
        }
      })
    }

    this.observer = new IntersectionObserver(observerCall, observerOptions)
    this.observer.observe(this.$refs.zeenOver)
  },
  destroyed () {
    delete this.observer
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
          if (target.dataset.zeenOver === 'mini') {
            const deltaX = e.clientX - offsetX
            const deltaY = e.clientY - offsetY

            if (deltaX >= this.margin && deltaX <= (window.innerWidth - coords.width - this.margin - 55)) {
              target.style.left = deltaX + 'px'
            }
            if (deltaY >= this.margin && deltaY <= (window.innerHeight - coords.height - this.margin)) {
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
  }
}
</script>

<style>
  :root {
    --zeen-over-icon-width: 20px;
    --zeen-over-icon-height: 20px;
    --zeen-over-button-width: 20px;
    --zeen-over-button-height: 20px
  }
</style>

<style lang="scss" scoped>
  .zeen-over {
    display: flex;
    width: 850px;
    height: 470px;
    background: #1E96ED;
    &_mini {
      width: var(--zeen-over-mini-width); //ширина задается в props
      height: calc((var(--zeen-over-mini-width)/16)*9);
      position: fixed;
      bottom: var(--zeen-over-mini-bottom);
      left: var(--zeen-over-mini-left);
      z-index: 1000;

      &_aspect_ratio_4_3 {
        height: calc((var(--zeen-over-mini-width)/4)*3);
      }
      &_aspect_ratio_1_1 {
        height: var(--zeen-over-mini-width);
      }
    }
  }

  .zeen-over_icon {
    width: var(--zeen-over-icon-width);
    height: var(--zeen-over-icon-height);
    position: fixed;
    top: auto!important;
    bottom: 30px;
    left: 30px!important;
    z-index: 1000;

    border-radius: 4px;
    cursor: pointer;
  }

  .zeen-over__button {
    width: var(--zeen-over-button-width);
    height: var(--zeen-over-button-height);
    margin-right: var(--zeen-over-button-width);
    position: absolute;
    top: -5px;
    right: -55px;

    border-top: 5px solid #1E96ED;
    border-right: 5px solid #1E96ED;
    transform: rotate(135deg);
    cursor: pointer;
  }
</style>
