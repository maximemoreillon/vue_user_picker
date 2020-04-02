<template>
  <div id="app">
    <UserPicker
      class="picker"
      apiUrl="http://172.16.98.151:8097"/>
  </div>
</template>

<script>
import UserPicker from './UserPicker.vue'

export default {
  name: 'App',
  components: {
    UserPicker
  }
}
</script>

<style scoped>
.picker{
  outline: 1px solid red;
  height: 200px;
}
</style>
