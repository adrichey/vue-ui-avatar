<template>
  <div id="app">
    <ui-avatar status="online" label="Alan Richey" image="http://localhost:8080/assets/guy.jpg" />
  </div>
</template>

<script>
import UIAvatar from './components/UIAvatar.vue'

export default {
  name: 'app',
  components: {
    'ui-avatar': UIAvatar,
  },
}
</script>

<style>
#app {
  margin: 40px;
}
</style>
