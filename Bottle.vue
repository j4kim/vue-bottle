<template>
  <svg
    style="width:240px;height:240px;paint-order:stroke"
    :viewBox="`0 0 ${size} ${size}`"
    ref="bottleSvg"
  >
    <mask id="bottlemask">
      <rect fill="black" :width="size" :height="size" />
      <path fill="white" :d="path" />
    </mask>
    <path stroke="black" stroke-alignment="outside" fill="white" :d="path" />
    <rect fill="black" :width="size" :height="y" :y="size - y" mask="url(#bottlemask)" />
  </svg>
</template>

<script>
import Hammer from 'hammerjs'
import { clamp } from 'lodash'

export default {
  data: () => ({
    path: 'M10,22A1,1 0 0,1 9,21V11C9,9 10,7.25 11,7V2.5A0.5,0.5 0 0,1 11.5,2H12.5A0.5,0.5 0 0,1 13,2.5V7C14,7.25 15,9 15,11V21A1,1 0 0,1 14,22H10Z',
    ratio: 0.5,
    size: 24
  }),

  computed: {
    y() {
      return this.size * this.ratio
    }
  },

  mounted () {
    let mc = Hammer(this.$el)
    mc.get('pan').set({ direction: Hammer.DIRECTION_VERTICAL })
    mc.on('panup pandown', e => this.add(e.velocityY))
  },

  methods: {
    add (velocity) {
      this.ratio = clamp(this.ratio - velocity/100, 0, 1)
    }
  }
}
</script>
