<template>
  <div>
    <div class="zeen-over" ref="zeenOver">
      <slot></slot>
    </div>

    <div
      class="zeen-over__mini"
      :class="{
        'zeen-over__mini_aspect_ratio_4_3': aspectRatio === '4:3',
        'zeen-over__mini_aspect_ratio_1_1': aspectRatio === '1:1',
      }"
      :style="{
        '--zeen-over-mini-bottom': margin + 'px',
        '--zeen-over-mini-left': margin + 'px',
      }"
      v-show="mini"
      @mousedown="move($event)"
      data-zeen-over="mini"
    >
      <span
        :class="['zeen-over__button', {
          'zeen-over__button_left': buttonLeft,
        }]"
        @click.stop="toIcon"
      >
        <svg
          width="26"
          height="14"
          viewBox="0 0 26 14"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="M0.292893 0.292893C0.683417 -0.0976311
            1.31658 -0.0976311 1.70711 0.292893L13
            11.5858L24.2929 0.292893C24.6834 -0.0976311
            25.3166 -0.0976311 25.7071 0.292893C26.0976
            0.683417 26.0976 1.31658 25.7071 1.70711L13.7071
            13.7071C13.3166 14.0976 12.6834 14.0976 12.2929
            13.7071L0.292893 1.70711C-0.0976311 1.31658
            -0.0976311 0.683417 0.292893 0.292893Z"
          />
        </svg>
      </span>
      <div class="zeen-over__slot" style="pointer-events: none">
        <slot></slot>
      </div>
    </div>

    <div class="zeen-over__icon" v-show="icon" @click="toIcon"></div>
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
      observer: null,
      buttonLeft: false
    }
  },
  mounted () {
    if (this.isActive) {
      this.observerFunc()
    }
  },
  destroyed () {
    delete this.observerFunc
    delete this.observer
  },
  methods: {
    observerFunc () {
      const observerOptions = {
        root: null,
        rootMargin: '0% 0% 100% 0%',
        threshold: 0
      }
      const observerCall = (entries) => {
        entries.forEach((entry) => {
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
        const maxHorizontal = document.documentElement.clientWidth - coords.width - this.margin
        const maxVertical = document.documentElement.clientHeight - coords.height - this.margin
        const iconWidth = target.children[0]?.clientWidth
        let deltaX
        let deltaY
        document.onmousemove = (e) => {
          if (target.dataset.zeenOver === 'mini') {
            deltaX = e.clientX - offsetX
            deltaY = e.clientY - offsetY
            if (
              deltaX >= this.margin &&
              deltaX <= maxHorizontal
            ) {
              target.style.left = deltaX + 'px'
            }
            if (
              deltaY >= this.margin &&
              deltaY <= maxVertical
            ) {
              target.style.top = deltaY + 'px'
              target.style.bottom = 'auto'
            }
            if (deltaX >= maxHorizontal - iconWidth) {
              this.buttonLeft = true
            } else {
              this.buttonLeft = false
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

<style lang="scss">
:root {
  /* Размеры */
  --zeen-over-width: 100%;
  --zeen-over-icon-size: 20px;
  --zeen-over-icon-border-radius: 4px;
  --zeen-over-button-size: 15px;
  --zeen-over-mini-width: 350px;

  /* Позиция */
  --zeen-over-icon-bottom: 30px;
  --zeen-over-icon-left: 30px;
  --zeen-over-button-rigth: -50px;
  --zeen-over-button-left: -50px;

  /* Цвета */
  --zeen-over-background: blue;
}
</style>

<style lang="scss" scoped>
.zeen-over {
  display: flex;
  width: var(--zeen-over-width);
  background: var(--zeen-over-background);

  &__icon {
    width: var(--zeen-over-icon-size);
    height: var(--zeen-over-icon-size);
    position: fixed;
    top: auto;
    bottom: var(--zeen-over-icon-bottom);
    left: var(--zeen-over-icon-left);
    z-index: 1000;
    background: var(--zeen-over-background);
    border-radius: var(--zeen-over-icon-border-radius);
    cursor: pointer;
  }

  &::after {
    content: '';
    display: block;
    width: 100%;
    padding-top: 56.25%;
  }

  &__mini {
    display: flex;
    background: var(--zeen-over-background);
    width: var(--zeen-over-mini-width);
    position: fixed;
    bottom: var(--zeen-over-mini-bottom);
    left: var(--zeen-over-mini-left);
    z-index: 1000;
    &::after {
      content: '';
      padding-top: 56.25%;
    }

    &_aspect_ratio_4_3 {
      &::after {
        padding-top: 75%;
      }
    }

    &_aspect_ratio_1_1 {
      &::after {
        padding-top: 100%;
      }
    }
  }
  &__slot {
    display: flex;
    width: 100%;
  }

  &__button {
    fill: var(--zeen-over-background);
    position: absolute;
    top: 0;
    right: var(--zeen-over-button-rigth);
    cursor: pointer;

    &_left {
      left: var(--zeen-over-button-left);
    }
  }
}
</style>
