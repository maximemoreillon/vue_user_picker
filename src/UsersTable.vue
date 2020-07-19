<template>
  <table class="users_table">

    <tr>
      <th></th>
      <th>名前 / Name</th>
      <th>立場 / Role</th>
    </tr>
    <tr
      class="user_row"
      v-for="user in users"
      v-bind:user="user"
      v-bind:key="user.identity.low"
      @:click="$emit('selection', $event)">

      <td class="avatar_cell">
        <img
          class="avatar"
          v-if="user.properties.avatar_src"
          v-bind:src="user.properties.avatar_src">

        <font-awesome-icon
          icon="user"
          v-else/>
      </td>

      <td>
        {{user_displayed_name(user)}}
      </td>

      <td>
        {{user.properties.role}}
      </td>

      <!-- Link to user profile -->
      <td>
        <a
          v-if="user_page_url(user)"
          v-bind:href="user_page_url(user)"
          v-on:click.stop>
          <font-awesome-icon
            icon="info-circle"/>
        </a>
      </td>







    </tr>

  </table>
</template>

<script>

import { library } from '@fortawesome/fontawesome-svg-core'

import {
  faUser,
  faInfoCircle,
} from '@fortawesome/free-solid-svg-icons'

import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

library.add(
  faUser,
  faInfoCircle
)

export default {
  name: 'User',
  components: {
    FontAwesomeIcon,
  },
  props: {
    users: Array,
  },
  methods: {
    user_displayed_name(user) {
      return user.properties.display_name
        || user.properties.name_kanji
        || user.properties.name
        || user.properties.full_name
        || user.properties.username
        || 'Unnamed user'
    },
    user_page_url(user) {
      if(process.env.VUE_APP_EMPLOYEE_MANAGER_FRONT_URL){
        return `${process.env.VUE_APP_EMPLOYEE_MANAGER_FRONT_URL}/?id=${user.identity.low}`
      }
      else return null

    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

table {
  width: 100%;
  text-align: left;
  border-collapse: collapse;
}

th, td {
  padding: 0.25em;

}

.user_row {
  border-top: 1px solid #dddddd;
  transition:
    color 0.25s,
    background-color 0.25s;
  cursor: pointer;
}

.user_row:hover{
  background-color: #eeeeee;
}

.user {
  cursor: pointer;

  display: flex;
  align-items: center;

  padding: 2px;

  transition:
    color 0.25s,
    background-color 0.25s;
}

.user > *:not(:last-child){
  margin-right: 0.5em;
}

.avatar {
  height: 1em;
  width: 1em;
  object-fit: contain;
}

.user:not(:last-child){
  border-bottom: 1px solid #dddddd;
}

.user:hover{
  background-color: #eeeeee;
}

.growing_spacer {
  flex-grow: 1;
}

a {
  text-decoration: none;
  color: currentColor;
}

a:hover {
  color: #c00000;
}

</style>
