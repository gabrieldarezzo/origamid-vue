<template>
  <div class="hello">

    <button @click="resultAPI">GeResultAPI</button>
    <div id="resultApi">
      <h2>Result - API</h2>
      <p>Company Name: {{CompanyName}}</p>
      <p>DailyListTimestamp: {{DailyListTimestamp | castDateToBr}}</p>
      <p>NotesforEachEntry: {{NotesforEachEntry}}</p>
      <p>StockAdjustmentFactor: {{StockAdjustmentFactor}}</p>
    </div>

  </div>
</template>

<script>

// https://api.iextrading.com/1.0/ref-data/daily-list/next-day-ex-date/sample
// CompanyName
// DailyListTimestamp (em pt-br)
// NotesforEachEntry
// StockAdjustmentFactor

import axios from 'axios'
import moment from 'moment'

export default {
  name: 'ApiIextrading',
  data: function () {
    return {
      CompanyName: '',
      DailyListTimestamp: '',
      NotesforEachEntry: '',
      StockAdjustmentFactor: ''
    }
  },
  props: {
    msg: String
  },
  created () {

  },
  filters: {
    castDateToBr: (stringDate) => {
      if (!stringDate) return ''
      return moment(stringDate).format('DD/MM/YYYY HH:SS')
    }
  },

  methods: {
    resultAPI () {
      const urlApi = 'https://api.iextrading.com/1.0/ref-data/daily-list/next-day-ex-date/sample'

      // async await
      axios.get(urlApi)
        .then((response) => {
          const json = response.data[0]

          // console.log(json.CompanyName)

          console.log(this)

          this.CompanyName = json.CompanyName
          this.DailyListTimestamp = json.DailyListTimestamp
          this.NotesforEachEntry = json.NotesforEachEntry
          this.StockAdjustmentFactor = json.StockAdjustmentFactor
        })
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
