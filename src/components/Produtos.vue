<template>
<div>

  <header class="header">
    <img class="logo" src="/assets/techno.svg" alt="Techno">
    <div class="carrinho_menu" @click="activeCart = true">{{carrinhoTotal | numberPricePtBr}} | {{carrinho.length}}</div>
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
        <button v-if="produto.estoque > 0" class="modal_btn" @click="addItem">Adicionar Item</button>
        <button v-if="produto.estoque == 0" class="modal_btn esgotado" disabled>Produto Esgotado</button>
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

  <section class="carrinho_modal" :class="{ativo : activeCart}" @click="closeCartModal">
    <div class="carrinho_container">
      <button class="carrinho_fechar" @click="activeCart = false">x</button>
      <h2 class="carrinho_titulo">Carrinho</h2>

      <div v-if="carrinho.length > 0">
        <ul class="carrinho_lista">
          <li v-for="(item, index) in carrinho" :key="item.id" class="carrinho_item">
            <p>{{item.nome}}</p>
            <p class="carrinho_preco">{{item.preco | numberPricePtBr}}</p>
            <button @click="removeItem(index)" class="carrinho_remover">X</button>
          </li>
        </ul>
        <p class="carrinho_total">{{carrinhoTotal | numberPricePtBr}}</p>
        <button class="carrinho_finalizar">Finalizar Compra</button>
      </div>
      <p v-if="carrinho.length == 0">O Carrinho está vazio.</p>
    </div>
  </section>

  <div class="alerta" :class="{ativo : showAlert}">
    <p class="alerta_mensagem">{{messageAlert}}</p>
  </div>
</div>
</template>

<script>

import axios from 'axios'
// import moment from 'moment'

export default {
  name: 'ApiIextrading',
  data: function () {
    return {
      carrinho: [],
      produto: {},
      produtos: [],
      messageAlert: 'Item Add',
      showAlert: false,
      activeCart: false

    }
  },
  created () {
    if (window.localStorage.carrinho) {
      this.carrinho = JSON.parse(window.localStorage.carrinho)
    }
    const urlApi = 'http://localhost:3000/produtos'
    axios.get(urlApi)
      .then((response) => {
        this.produtos = response.data
      })

    this.router()
  },
  computed: {
    carrinhoTotal () {
      let subTotal = 0

      if (this.carrinho.length === 0) {
        return 0
      }

      this.carrinho.map((item) => {
        subTotal += item.preco
      })
      return subTotal
    }
  },
  methods: {
    fetchProduct (id) {
      const urlApi = `http://localhost:3000/produtos/${id}`
      axios.get(urlApi)
        .then((response) => {
          this.produto = response.data
        })
    },
    router () {
      const hash = document.location.hash
      if (hash) {
        this.fetchProduct(hash.replace('#', ''))
      }
    },
    closeModal ({ target, currentTarget }) {
      if (target === currentTarget) this.produto = {}
    },
    closeCartModal ({ target, currentTarget }) {
      if (target === currentTarget) this.activeCart = false
    },
    compareStock () {
      const itens = this.carrinho.filter(item => {
        console.log(item.id)
        if (item.id === this.produto.id) {
          return true
        }
      })
      this.produto.estoque = this.produto.estoque - itens.length
    },
    addItem () {
      this.produto.estoque--
      const { id, nome, preco } = this.produto

      // avoid [Vue warn]: Duplicate keys detected: ''. happens when {id is same}

      this.carrinho.push({
        id,
        nome,
        preco
      })

      this.alertMessage(`${nome} Adicionado ao carrinho`)
    },
    removeItem (index) {
      this.carrinho.splice(index, 1)
    },
    openModal (id) {
      this.fetchProduct(id)
      window.scrollTo({
        top: 0,
        behavior: 'smooth'
      })
    },
    alertMessage (message) {
      this.messageAlert = message
      this.showAlert = true

      window.setInterval(() => {
        this.showAlert = false
      }, 2000)
    }
  },
  filters: {
    numberPricePtBr (value) {
      if (!value) {
        let auxValue = '0'
        auxValue.toLocaleString('pt-BR', {
          style: 'currency',
          currency: 'BRL'
        })
      }
      return value.toLocaleString('pt-BR', {
        style: 'currency',
        currency: 'BRL'
      })
    },
    capitalize (value) {
      if (typeof value !== 'string') return ''
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  },

  watch: {
    produto () {
      document.title = this.produto.nome || 'Techno'
      const hash = this.produto.id || ''
      history.pushState(null, null, `#${hash}`)

      if (this.produto) {
        this.compareStock()
      }
    },

    carrinho () {
      // console.log(this.carrinho)
      // console.log(JSON.stringify(this.carrinho))
      window.localStorage.carrinho = JSON.stringify(this.carrinho)
    }

  }

}
</script>

<style scoped>

</style>
