<template>
  <svg
    :style="{
      width: `${size/2}px`,
      height: `${size}px`,
      paintOrder: 'stroke'
    }"
    viewBox="0 0 12 24"
    ref="bottleSvg"
  >
    <mask id="bottlemask">
      <rect fill="black" width="12" height="24" />
      <path fill="white" :d="path" />
    </mask>
    <path stroke="black" stroke-alignment="outside" fill="white" :d="path" />
    <rect
      fill="black"
      width="12"
      :height="y"
      :y="24 - y"
      mask="url(#bottlemask)"
    />
  </svg>
</template>

<script>
import Hammer from 'hammerjs'
import { clamp } from 'lodash'

export default {
  props: {
    value: {
      type: [Number, String],
      default: 0
    },
    size: {
      type: [Number, String],
      default: 120
    },
    speed: {
      type: Number,
      default: 0.01
    }
  },

  data: () => ({
    path: "M3.72,23.39a1.14,1.14,0,0,1-1.14-1.13V10.86c0-2.28,1.14-4.27,2.28-4.56V1.17A.57.57,0,0,1,5.43.61H6.57a.57.57,0,0,1,.57.56V6.3c1.14.29,2.28,2.28,2.28,4.56v11.4a1.14,1.14,0,0,1-1.14,1.13Z",
  }),

  computed: {
    y () {
      return 24 * this.value
    }
  },

  mounted () {
    let mc = Hammer(this.$el)
    mc.get('pan').set({ direction: Hammer.DIRECTION_VERTICAL })
    mc.on('panup pandown', e => this.add(e.velocityY))
  },

  methods: {
    add (velocity) {
      let newValue = clamp(this.value - velocity * this.speed, 0, 1)
      this.$emit('input', newValue)
    }
  }
}
</script>
