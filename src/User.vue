<template>
  <div class="user">
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

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>


.user {
  cursor: pointer;
  margin: 1px;

  transition: color 0.25s;
}

.user:hover{
  color: #c00000;
}

</style>
