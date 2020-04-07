<template>
  <div class="user"
    v-on:click="$emit('selection', user)">

    <img
      class="avatar"
      v-if="user.properties.avatar_src"
      v-bind:src="user.properties.avatar_src">
    <img
      v-else
      class="avatar"
      src="@/assets/account.png">

    <div class="">
      {{user.properties.name_kanji}}
    </div>

   </div>
</template>

<script>

export default {
  name: 'User',
  components: {

  },
  props: {
    user: Object,
  },
  data(){
    return{

    }
  },
  mounted(){

  },
  methods: {

  },
  computed: {

  }
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
  margin-right: 5px;
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

</style>
