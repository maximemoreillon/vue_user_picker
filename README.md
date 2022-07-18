# User picker

[![npm version](https://badge.fury.io/js/@moreillon%2Fvue_user_picker.svg)](https://badge.fury.io/js/@moreillon%2Fvue_user_picker)

A Vue.js component to select members from groups managed by the [group management microservice](https://github.com/maximemoreillon/group_manager)

## Usage

```
<template>
  <div id="app">
    <UserPicker
      :groupManagerApiUrl="https://api.groups.example.com"
      @selection="selection($event)"/>
  </div>
</template>

<script>
import UserPicker from '@moreillon/vue_user_picker'

export default {
  name: 'App',
  components: {
    UserPicker
  },
  methods: {
    selection({display_name}){
      alert(`You clicked ${display_name}`)
    }
  }
}
</script>
```

