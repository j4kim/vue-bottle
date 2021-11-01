<template>
  <main>
    <bottle v-model="ratio"/>
    <input type="number" v-model="ratio" min="0" max="1" step="0.02">
  </main>
</template>

<script>
import Bottle from './Bottle'

export default {
  components: { Bottle },

  data: () => ({
    ratio: 0.5
  })
}
</script>
