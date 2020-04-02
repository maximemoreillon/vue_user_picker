<template>
  <div class="user_picker">

    <div class="groups_container">
      <template v-if="!groups_loading">
        <Group
          v-for="group in groups"
          v-bind:group="group"
          v-bind:key="group.identity.low"
          v-bind:apiUrl="apiUrl"
          v-on:nodeSelected="get_employees_belonging_to_node($event)"/>
      </template>
      <loader v-else />
    </div>

    <div class="users_container">
      <template v-if="!users_loading">
        <User
          v-for="user in users"
          v-bind:user="user"
          v-bind:key="user.identity.low"
          v-on:click="$emit('selection',employee)"/>
      </template>
      <loader v-else />
    </div>

  </div>
</template>

<script>

import axios from 'axios'
//import VueCookies from 'vue-cookies'
import Group from './Group.vue'
import User from './User.vue'

import Loader from '@moreillon/vue_loader'

export default {
  name: 'UserPicker',
  props: {
    apiUrl: String,
  },
  components: {
    Group,
    User,

    Loader
  },
  data(){
    return {
      groups: [],
      groups_loading: false,

      users: [],
      users_loading: false,
    }
  },
  mounted(){
    this.get_highest_hierarchy_groups()
  },
  methods: {
    get_highest_hierarchy_groups(){
      this.groups_loading = true;

      axios.post(`${this.apiUrl}/get_highest_hierarchy_groups`, {})
      .then(response => {
        this.groups.splice(0,this.groups.length)
        response.data.forEach((record) => {
          let group = record._fields[record._fieldLookup['group']]
          this.groups.push(group)
        });

      })
      .catch( (error) => { console.log(error) })
      .finally( () => { this.groups_loading = false })
    },

    get_employees_belonging_to_node(group){
      this.users_loading = true;
      axios.post(`${this.apiUrl}/get_users_of_group`, {group_id: group.identity.low})
      .then(response => {
        this.users.splice(0,this.users.length)
        response.data.forEach((record) => {
          let user = record._fields[record._fieldLookup['employee']]
          this.users.push(user)
        });
      })
      .catch(error => console.log(error))
      .finally( () => { this.users_loading = false })
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.user_picker {
  display: flex;
}

.user_picker > div {
  /* share space horizontally */
  flex-grow: 1;
  flex-shrink: 1;
  flex-basis: 0;

  height: 100%;
  overflow-y: auto;
}
</style>
