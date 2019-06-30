<template>
  <div class="hello">
    <p>Nome Completo: {{fullName}}</p>
    <p>Capital: {{capital}}</p>

    <div>
      <select name="" id="" v-model="pais">
        <option v-for="pais in paises" :key="pais.name" :value="pais.name">{{pais.name}}</option>
      </select>
    </div>

  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'computed',
  data: function () {
    return {
      name: 'Gabriel',
      lastName: 'Darezzo',
      paises: [],
      pais: {},
      capital: ''
    }
  },

  // async await
  created () {
    axios.get('http://restcountries.eu/rest/v2/all')
      .then((response) => {
        this.paises = response.data
      })
  },
  computed: {
    // uma função "getter" computada (computed getter)
    fullName: function () {
      return this.name + ' ' + this.lastName
    }
  },
  watch: {
    pais (valor) {
      this.capital = this.paises.find(pais => pais.name === valor).capital
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
