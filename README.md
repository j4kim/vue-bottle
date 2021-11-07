# vue-bottle

A custom input for Vue featuring a bottle of wine.

## Install

```
npm i vue-bottle
```

## Usage

```vue
<template>
  <main>
    <bottle v-model="ratio"/>
  </main>
</template>

<script>
import Bottle from 'vue-bottle'

export default {
  components: { Bottle },

  data: () => ({
    ratio: 0.5
  })
}
</script>
```

## Demo

https://codesandbox.io/s/vue-bottle-demo-rhnjf
