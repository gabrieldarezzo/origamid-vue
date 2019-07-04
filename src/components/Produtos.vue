<template>
  <section>

    <!--
      <img :src="test" />
      <img :src="test" />
    -->

    <hr>
    <div v-for="produto in produtos" :key="produto.id" class="produto">
      <img :src="produto.img" />
        {{produto.nome}}
    </div>

  </section>
</template>

<script>

import axios from 'axios'
// import moment from 'moment'

export default {
  name: 'ApiIextrading',
  data: function () {
    return {
      produtos: [],
      // test: require('@/assets/test.jpg')
      // test: require('../assets/images/notebook/notebook.jpg')
      test: require('@/assets/images/notebook/notebook.jpg')

    }
  },
  created () {
    const urlApi = 'http://localhost:3000/produtos'

    axios.get(urlApi)
      .then((response) => {
        this.produtos = response.data
      })
  },
  methods: {
    resultAPI () {}
  }

}
</script>

<style scoped>

</style>
