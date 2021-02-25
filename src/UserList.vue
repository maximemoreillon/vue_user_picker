<template>
  <div class="user_list">

    <div
      class="user"
      v-for="user in users"
      v-bind:key="JSON.stringify(user.identity)"
      @click="$emit('selection', user)">

      <div class="avatar_wrapper">
        <img
          class="avatar"
          v-if="user.properties.avatar_src"
          v-bind:src="user.properties.avatar_src">

        <font-awesome-icon
          icon="user"
          v-else/>
      </div>
      <div class="name">
        {{user_displayed_name(user)}}
      </div>
      <div class="" v-if="user.properties.role">
        ({{user.properties.role}})
      </div>

      <div class="spacer" />

      <div
        class="profile_link"
        v-if="userManagerFrontUrl"
        v-on:click.stop="view_profile(user)">

        <font-awesome-icon
          icon="external-link-alt"/>
      </div>


    </div>


  </div>

</template>

<script>

import { library } from '@fortawesome/fontawesome-svg-core'

import {
  faUser,
  faExternalLinkAlt,

} from '@fortawesome/free-solid-svg-icons'

import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

library.add(
  faUser,
  faExternalLinkAlt
)

export default {
  name: 'User',
  components: {
    FontAwesomeIcon,
  },
  props: {
    users: Array,
    userManagerFrontUrl: {
      type: String,
      default(){
        return process.env.VUE_APP_USER_MANAGER_FRONT_URL
          || process.env.VUE_APP_EMPLOYEE_MANAGER_FRONT_URL
      }
    }
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

    view_profile (user) {
      const user_id = user.identity.low || user.identity
      const url = `${this.userManagerFrontUrl}/users/${user_id}`
      window.open(url)
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.user {
  display: flex;
  align-items: center;
  padding: 0.25em;
  cursor: pointer;

}

.user:hover{
  background-color: #eeeeee;
}
.user:not(:last-child) {
  border-bottom: 1px solid #dddddd;
}

.user > *:not(:last-child) {
  margin-right: 1em;
}
.spacer {
  flex-grow: 1;
}

.avatar {
  height: 1em;
  width: 1em;
  object-fit: contain;
}

.profile_link {
  text-decoration: none;
  color: currentColor;
}

.profile_link:hover {
  color: #c00000;
}
</style>
