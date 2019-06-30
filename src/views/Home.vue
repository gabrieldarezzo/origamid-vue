<template>
  <div class="home">

    <ApiIextrading />
    <!--
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    -->
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import ApiIextrading from '@/components/ApiIextrading.vue'

export default {
  name: 'home',
  components: {
    // HelloWorld
    ApiIextrading
  }
}
</script>
