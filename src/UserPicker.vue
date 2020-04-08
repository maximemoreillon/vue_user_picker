<template>
  <div class="user_picker">

    <GroupPicker
      class="node_container groups_container"
      v-on:selection="get_users_of_group($event)"
      v-bind:apiUrl="apiUrl"
      usersWithNoGroup/>


    <div class="node_container users_container">
      <template v-if="!users_loading">
        <User
          v-for="user in users"
          v-bind:user="user"
          v-bind:key="user.identity.low"
          v-on:selection="$emit('selection', $event)"/>
      </template>

      <!-- wrapper loader so it can be centered in the div -->
      <div class="loader_wrapper" v-else>
        <loader />
      </div>

    </div>

  </div>
</template>

<script>

import axios from 'axios'
//import VueCookies from 'vue-cookies'
import User from './User.vue'

import GroupPicker from '@moreillon/vue_group_picker'
import Loader from '@moreillon/vue_loader'

export default {
  name: 'UserPicker',
  props: {
    apiUrl: String,
  },
  components: {
    GroupPicker,
    User,

    Loader
  },
  data(){
    return {

      users: [],
      users_loading: false,
    }
  },
  mounted(){},
  methods: {

    get_users_of_group(group){
      this.users_loading = true;
      if(group){
        axios.get(`${this.apiUrl}/users_of_group`, {
          params: {id: group.identity.low}
        })
        .then(response => {
          this.users.splice(0,this.users.length)
          response.data.forEach((record) => {
            let user = record._fields[record._fieldLookup['user']]
            this.users.push(user)
          });
        })
        .catch(error => console.log(error.response.data))
        .finally( () => { this.users_loading = false })
      }
      else {
        axios.get(`${this.apiUrl}/users_with_no_group`)
        .then(response => {
          this.users.splice(0,this.users.length)
          response.data.forEach((record) => {
            let user = record._fields[record._fieldLookup['user']]
            this.users.push(user)
          });
        })
        .catch(error => console.log(error.response.data))
        .finally( () => { this.users_loading = false })
      }

    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.user_picker {
  display: flex;
  flex-wrap: wrap;
}

.node_container {
  /* share space horizontally */
  flex-grow: 1;
  flex-shrink: 1;
  flex-basis: 0;

  height: 100%;
  overflow-y: auto;

  min-width: 300px;

  border: 1px solid #dddddd;
}

.loader_wrapper {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 200%;
}
</style>
