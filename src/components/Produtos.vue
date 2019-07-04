<template>
<div>

  <header class="header">
    <img class="logo" src="/assets/techno.svg" alt="Techno">
    <div class="carrinho_menu">{{carrinhoTotal}} | </div>
  </header>

  <section class="modal" v-if="produto.id" @click="closeModal">
    <div class="modal_container">
      <div class="modal_img">
        <img :src="produto.img" :alt="produto.nome"  />
      </div>

      <div class="modal_dados">
        <button class="modal_fechar" @click="closeModal">X</button>
        <span class="modal_preco">{{produto.preco | numberPricePtBr}}</span>
        <h2 class="modal_titulo">{{produto.nome | capitalize}}</h2>
        <p>{{produto.descricao}}</p>
        <button class="modal_btn">Adicionar Item</button>
      </div>

      <div class="avaliacoes">
        <h2 class="avaliacoes_subtitulo">Avaliações</h2>
        <ul>
          <li v-for="avaliacao in produto.avaliacoes" :key="avaliacao.nome" class="avaliacao">
            <p class="avaliacao_descricao">{{avaliacao.descricao}}</p>
            <p class="avaliacao_usuario">{{avaliacao.nome}} | {{avaliacao.estrelas}} estrelas </p>
          </li>
        </ul>

      </div>
    </div>
  </section>

  <section class="produtos" @click="closeModal">
    <div @click="openModal(item.id)" v-for="item in produtos" :key="item.id" class="produto">
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
      produtos: [],
      carrinhoTotal: 0
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
    },
    closeModal ({ target, currentTarget }) {
      if (target === currentTarget) this.produto = {}
    },
    openModal (id) {
      this.fetchProduct(id)
      window.scrollTo({
        top: 0,
        behavior: 'smooth'
      })
    }

  },
  filters: {
    numberPricePtBr (value) {
      if (!value) return ''

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
