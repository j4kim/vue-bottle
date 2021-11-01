<template>
  <svg
    :style="{
      width: `${size}px`,
      height: `${size}px`,
      paintOrder: 'stroke'
    }"
    :viewBox="`0 0 ${boxSize} ${boxSize}`"
    ref="bottleSvg"
  >
    <mask id="bottlemask">
      <rect fill="black" :width="boxSize" :height="boxSize" />
      <path fill="white" :d="path" />
    </mask>
    <path stroke="black" stroke-alignment="outside" fill="white" :d="path" />
    <rect fill="black" :width="boxSize" :height="y" :y="boxSize - y" mask="url(#bottlemask)" />
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
    }
  },

  data: () => ({
    path: 'M10,22A1,1 0 0,1 9,21V11C9,9 10,7.25 11,7V2.5A0.5,0.5 0 0,1 11.5,2H12.5A0.5,0.5 0 0,1 13,2.5V7C14,7.25 15,9 15,11V21A1,1 0 0,1 14,22H10Z',
    boxSize: 24
  }),

  computed: {
    y () {
      return this.boxSize * this.value
    }
  },

  mounted () {
    let mc = Hammer(this.$el)
    mc.get('pan').set({ direction: Hammer.DIRECTION_VERTICAL })
    mc.on('panup pandown', e => this.add(e.velocityY))
  },

  methods: {
    add (velocity) {
      let newValue = clamp(this.value - velocity/100, 0, 1)
      this.$emit('input', newValue)
    }
  }
}
</script>
