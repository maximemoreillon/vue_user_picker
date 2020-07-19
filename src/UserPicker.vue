<template>
  <div class="user_picker">

    <GroupPicker
      class="node_container groups_container"
      v-on:selection="get_users_of_group($event)"
      v-bind:apiUrl="apiUrl"
      v-bind:groupPageUrl="groupPageUrl"
      usersWithNoGroup/>


    <div class="node_container users_container">

      <UsersTable
        v-if="!users_loading && selected_group !== undefined"
        :users="users"
        :selected_group="selected_group"
        @selection="$emit('selection',$event)"/>

      <div
        class="no_selection_wrappper"
        v-if="!users_loading && selected_group === undefined">
        Group not selected
      </div>

      <!-- wrapper loader so it can be centered in the div -->
      <div
        class="loader_wrapper"
        v-if="users_loading">
        <loader />
      </div>

    </div>

  </div>
</template>

<script>

import axios from 'axios'
import UsersTable from './UsersTable.vue'

import GroupPicker from '@moreillon/vue_group_picker'
import Loader from '@moreillon/vue_loader'

export default {
  name: 'UserPicker',
  props: {
    apiUrl: {
      type: String,
      default(){return process.env.VUE_APP_GROUP_MANAGER_API_URL}
    },
    groupPageUrl: String,
  },
  components: {
    GroupPicker,
    UsersTable,

    Loader
  },
  data(){
    return {

      users: [],
      users_loading: false,
      selected_group: undefined,
    }
  },
  mounted(){},
  methods: {

    get_users_of_group(group){
      this.users_loading = true
      this.selected_group = group

      let group_id = 'none'
      if(group) group_id = group.identity.low

      axios.get(`${this.apiUrl}/groups/${group_id}/members`)
      .then(response => {
        this.users.splice(0,this.users.length)
        response.data.forEach((record) => {
          let user = record._fields[record._fieldLookup['user']]
          this.users.push(user)
        });
      })
      .catch(error => console.log(error.response.data))
      .finally( () => { this.users_loading = false })

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

.no_selection_wrappper{
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
