<template>
  <div class='Main'>
    <dispatcher></dispatcher>
  </div>
</template>

<script>
import dispatcher from 'lay/Dispatcher';
export default {
  name: 'Main',
  data() {
    return {}
  },
  components: {
    dispatcher
  }
}
</script>
<style scoped lang='stylus'>
.Main
  width 100vw
  height 100vh
</style>
