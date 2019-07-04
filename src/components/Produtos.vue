<template>
<div>

  <section class="modal">
    {{produto}}
  </section>

  <section class="produtos">
    <div @click="fetchProduct(item.id)" v-for="item in produtos" :key="item.id" class="produto">
      <img :src="item.img" :alt="produto.nome" class="produto_img" />
      <div class="produto_info">
        <span class="produto_preco">{{item.preco | numberPricePtBr}}</span>
        <h2 class="produto_titulo">{{item.nome | capitalize}}</h2>
      </div>
    </div>
  </section>
</div>
</template>

<script>

import axios from 'axios'
// import moment from 'moment'

export default {
  name: 'ApiIextrading',
  data: function () {
    return {
      produto: {},
      produtos: []
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
    fetchProduct (id) {
      const urlApi = `http://localhost:3000/produtos/${id}`
      axios.get(urlApi)
        .then((response) => {
          this.produto = response.data
        })
    }
  },
  filters: {
    numberPricePtBr (value) {
      return value.toLocaleString('pt-BR', {
        style: 'currency',
        currency: 'BRL'
      })
    },
    capitalize (value) {
      if (typeof value !== 'string') return ''
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  }

}
</script>

<style scoped>

</style>
