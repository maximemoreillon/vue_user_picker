<template>
  <div class="UserPicker">
    <template v-if="!loading">
      <Group
        v-for="group in groups"
        v-bind:group="group"
        v-bind:key="group.identity.low"
        v-bind:apiUrl="api_url"
        v-on:nodeSelected="get_employees_belonging_to_node($event)"/>
    </template>
    <loader v-else />
  </div>
</template>

<script>

import axios from 'axios'
//import VueCookies from 'vue-cookies'
import Group from './Group.vue'
import Loader from '@moreillon/vue_loader'

export default {
  name: 'UserPicker',
  props: {

  },
  components: {
    Group,
    Loader
  },
  data(){
    return {
      api_url: 'http://172.16.98.151:8097',
      groups: [],
      loading: false,
    }
  },
  mounted(){
    this.get_highest_hierarchy_groups()
  },
  methods: {
    get_highest_hierarchy_groups(){
      this.loading = true;

      axios.post(`${this.api_url}/get_highest_hierarchy_groups`, {})
      .then(response => {
        this.groups.splice(0,this.groups.length)
        response.data.forEach((record) => {
          let group = record._fields[record._fieldLookup['group']]
          this.groups.push(group)
        });

      })
      .catch( (error) => { console.log(error) })
      .finally( () => { this.loading = false })
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
