<template>
  <div id="app">
    <UserPicker
      class="picker"
      @selection="selection($event)"/>
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
      const display_name = data.properties.display_name
      alert(`You clicked ${display_name} (ID: ${data.identity})`)
    }
  }
}
</script>

<style scoped>
.picker{
  height: 400px;
}
</style>
