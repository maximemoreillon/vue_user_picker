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

        <AccountIcon
          class="avatar"
          v-else/>

      </div>

      <div class="name">{{user_displayed_name(user)}}</div>
      <div class="role" v-if="user.role">({{user.role}})</div>
      

      <div class="spacer" />


      <OpenInNewIcon
        class="profile_link"
        v-if="userManagerFrontUrl"
        @click.stop="view_profile(user)"/>


    </div>


  </div>

</template>

<script>

import AccountIcon from 'vue-material-design-icons/Account.vue';
import OpenInNewIcon from 'vue-material-design-icons/OpenInNew.vue';


export default {
  name: 'User',
  components: {
    AccountIcon,
    OpenInNewIcon
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
  gap: 1em;
  padding: 0.25em;
  cursor: pointer;

  transition: background-color 0.25s;

}

.user:hover{
  background-color: #eeeeee;
}



.user_list > *+* {
  border-top: 1px solid #dddddd;
}


.spacer {
  flex-grow: 1;
}


.material-design-icon {
  display: flex;
  align-items: center;
}

.name {
  max-width: 30%;
  white-space: nowrap;
  overflow-x: hidden;
  text-overflow: ellipsis;

}

.role {
  max-width: 30%;
  white-space: nowrap;
  overflow-x: hidden;
  text-overflow: ellipsis;

}

.avatar {
  height: 1.5em;
  width: 1.5em;
  object-fit: contain;
}

.profile_link {
  text-decoration: none;
  color: currentColor;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 1em;
  width: 1em;
  opacity: 0;
  transition:
    color 0.25s,
    opacity 0.25s;
}

.user:hover .profile_link {
  opacity: 1;
}

.profile_link:hover {
  color: #c00000;
}

</style>
