<template>
  <div class="user_picker">
    <GroupPicker
      class="picker_container groups_container"
      @selection="get_users_of_group($event)"
      :selectedGroupId="selectedGroupId"
      :groupManagerApiUrl="groupManagerApiUrl"
      :groupManagerFrontUrl="groupManagerFrontUrl"
      usersWithNoGroup
    />

    <div class="picker_container users_container">
      <template v-if="!users_loading && selected_group && !error">
        <div class="search_wrapper">
          <input placeholder="Search users" type="text" v-model="search" />
          <MagnifyIcon class="search" />
        </div>
        <UserList
          :users="filtered_users"
          :selected_group="selected_group"
          :userManagerFrontUrl="userManagerFrontUrl"
          @selection="$emit('selection', $event)"
        />
      </template>

      <div
        class="no_selection_wrappper"
        v-if="!users_loading && !selected_group && !error"
      >
        Group not selected
      </div>

      <!-- wrapper loader so it can be centered in the div -->
      <div class="loader_wrapper" v-if="users_loading && !error">
        <loader />
      </div>

      <div class="error_wrapper" v-if="!users_loading && error">
        <span class="error">Error while fetching users</span>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios"
import VueCookies from "vue-cookies"

import UserList from "./UserList.vue"

import GroupPicker from "@moreillon/vue_group_picker"
import Loader from "@moreillon/vue_loader"

import MagnifyIcon from "vue-material-design-icons/Magnify.vue"

export default {
  name: "UserPicker",
  props: {
    groupManagerApiUrl: {
      type: String,
      default() {
        return process.env.VUE_APP_GROUP_MANAGER_API_URL
      },
    },
    groupManagerFrontUrl: {
      type: String,
      default() {
        return process.env.VUE_APP_GROUP_MANAGER_FRONT_URL
      },
    },
    userManagerFrontUrl: {
      type: String,
      default() {
        return (
          process.env.VUE_APP_USER_MANAGER_FRONT_URL ||
          process.env.VUE_APP_EMPLOYEE_MANAGER_FRONT_URL ||
          process.env.VUE_APP_GROUP_MANAGER_FRONT_URL
        )
      },
    },
  },
  components: {
    GroupPicker,
    UserList,
    MagnifyIcon,
    Loader,
  },
  data() {
    return {
      users: [],
      users_loading: false,
      error: null,
      selected_group: undefined,
      search: "",
    }
  },
  mounted() {
    const jwt = VueCookies.get("jwt")
    if (jwt && !axios.defaults.headers.common.Authorization) {
      axios.defaults.headers.common["Authorization"] = `Bearer ${jwt}`
    }
  },
  methods: {
    get_users_of_group(group) {
      this.users_loading = true
      this.selected_group = group

      const group_id = group._id

      const url = `${this.groupManagerApiUrl}/v3/groups/${group_id}/members`
      const params = { batch_size: -1 }

      axios
        .get(url, { params })
        .then(({ data }) => {
          this.users = data.items
        })
        .catch((error) => {
          if (error.response) console.error(error.response.data)
          else console.error(error)
          this.error = `Error while fetching users`
        })
        .finally(() => {
          this.users_loading = false
        })
    },
  },
  computed: {
    filtered_users() {
      if (!this.search) return this.users

      const keys_to_check = [
        "display_name",
        "name_kanji",
        "name",
        "full_name",
        "username",
        "name_romaji",
        "email_address",
      ]

      return this.users.filter((user) => {
        return keys_to_check.find((key) => {
          const value = user[key]
          if (!value) return false
          return value.toLowerCase().includes(this.search.toLowerCase())
        })
      })
    },
    selectedGroupId() {
      if (!this.selected_group) return null
      return this.selected_group._id
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.user_picker {
  display: flex;
  align-items: stretch;
}

.picker_container {
  /* share space horizontally */
  flex-grow: 1;
  flex-shrink: 0;
  flex-basis: 0;

  overflow-y: auto;

  border: 1px solid #dddddd;
}

.picker_container:last-child {
  margin: 0;
  margin-left: 0.5em;
}

.loader_wrapper {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 200%;
}

.no_selection_wrappper {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.error_wrapper {
  text-align: center;
  justify-content: center;
  padding: 1em;
}
.error {
  color: #c00000;
}

.search_wrapper {
  display: flex;
  align-items: center;
  margin: 1em 0.5em;
  margin-bottom: 1em;
}

.search_wrapper input {
  background-color: inherit;
  color: inherit;
  margin-right: 0.5em;
  padding: 0.5em 0.25em;
  flex-grow: 1;
  border: none;
  border-bottom: 1px solid #444444;
}

@media screen and (max-width: 600px) {
  .user_picker {
    flex-direction: column;
  }
  .picker_container:last-child {
    margin: 0;
    margin-top: 0.5em;
  }
}
</style>
