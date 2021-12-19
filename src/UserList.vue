<template>
  <div class="user_list">

    <div
      class="user"
      v-for="(user, index) in users"
      v-bind:key="`user_${index}`"
      @click="$emit('selection', user)">

      <div class="avatar_wrapper">
        <img
          class="avatar"
          v-if="user.avatar_src"
          v-bind:src="user.avatar_src">

        <font-awesome-icon
          icon="user"
          v-else/>
      </div>

      <div class="name">{{user_displayed_name(user)}}</div>
      <div class="" v-if="user.role">({{user.role}})</div>

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
    get_id_of_item(item){
      return item._id
    },
    user_displayed_name(user) {
      return user.display_name
        || user.name_kanji
        || user.name
        || user.full_name
        || user.username
        || 'Unnamed user'
    },

    view_profile (user) {
      const user_id = this.get_id_of_item(user)
      const url = `${this.userManagerFrontUrl}/users/${user_id}`
      window.open(url, '_blank')
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
