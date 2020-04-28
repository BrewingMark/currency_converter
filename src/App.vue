<template lang="html">
  <div>
    <form v-on:submit.prevent>


      <h1>convert from euro</h1>
      <label>Enter amount: </label>
      <input type="number" v-model.number="enteredAmount">
      <label>Currency: </label>
      <select v-model="selectedRate">
        <option v-for="(value, key) in rates" :value="value" :key="key">{{key}}</option>
      </select>
      <div>
        <h3 v-if="selectedRate">You will get: {{convertFromEuro | toDecimals()}}</h3>
      </div>
      <button type="button" v-on:click="saveConversion">Save</button>


      <h1>convert to euro</h1>
      <label>Enter amount: </label>
      <input type="number" v-model.number="enteredAmount2">
      <h3 v-if="selectedRate">You will get: {{convertToEuro | toDecimals()}}</h3>
      <button type="button" v-on:click="saveConversion">Save</button>
      <h1>Two way conversion</h1>
      <label>choose convert from</label>
      <select v-model="convertFrom">
        <option v-for="(value, key) in rates" :value="value" :key="key" >{{key}}</option>
      </select>
      <input type="number" v-model.number="fromAmount">
      <select v-model="convertTo">
        <option v-for="(value, key) in rates" :value="value" :key="key" >{{key}}</option>
      </select>
      <h3>result: {{twoWayConvert | toDecimals}}</h3>
      <button type="button" v-on:click="saveConversion">Save</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'app',
  data(){
    return {
      rates: {},
      enteredAmount: null,
      enteredAmount2: null,
      selectedRate: null,
      convertFrom: null,
      convertTo: null,
      fromAmount: null,
      savedResults: []
    }
  },
  computed: {
    convertFromEuro() {
      return(this.enteredAmount * this.selectedRate)
    },
    convertToEuro() {
      return (this.enteredAmount2 / this.selectedRate)
    },
    twoWayConvert(){
      let euroValue = (this.fromAmount / this.convertFrom);
      let result = (euroValue * this.convertTo);
      return result;
    }
  },
  filters: {
    toDecimals(value) {
      return value.toFixed(2)
    }
  },
  methods: {
    getRates(){
      fetch('https://api.exchangeratesapi.io/latest')
      .then(res => res.json())
      .then(data => this.rates = data.rates)
    },
  },
  mounted() {
    this.getRates();
  }
}

</script>

<style lang="css" scoped>
</style>
