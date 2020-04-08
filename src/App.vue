<template>
  <div id="app">
    <UserPicker
      class="picker"
      apiUrl="http://172.16.98.151:7000"
      v-on:selection="selection($event)"/>
  </div>
</template>

<script>
import UserPicker from './UserPicker.vue'

export default {
  name: 'App',
  components: {
    UserPicker
  },
  methods: {
    selection(data){
      console.log(data)
    }
  }
}
</script>

<style scoped>
.picker{
  height: 200px;
}
</style>
