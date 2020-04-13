<template>
  <div class="user"
    v-on:click="$emit('selection', user)">

    <img
      class="avatar"
      v-if="user.properties.avatar_src"
      v-bind:src="user.properties.avatar_src">

    <font-awesome-icon
      icon="user"
      v-else/>

    <div class="">
      {{user.properties.name_kanji}}
    </div>

    <div class="growing_spacer"/>

    <a
      v-if="userPageUrl"
      v-bind:href="userPageUrl"
      v-on:click.stop>
      <font-awesome-icon
        icon="info-circle"/>
    </a>



   </div>
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
    user: Object,
    userPageUrl: String,
  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>


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
