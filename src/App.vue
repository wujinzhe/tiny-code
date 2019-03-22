<template>
  <div id="app">
    <codemirror></codemirror>
  </div>
</template>

<script>
import codemirror from '@/components/code.vue'

export default {
  name: 'App',
  components: {
    codemirror
  }
}
</script>

<style>
* {
  padding: 0;
  margin: 0;
}
</style>
