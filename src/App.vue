<template>
  <div id="app">
    <h1>Energy Mix For United Kingdom</h1>
    <energy-chart :energyMix="energyMix"></energy-chart>
  </div>
</template>

<script>

import EnergyChart from './components/EnergyChart.vue'
export default {
  name: 'app',
  data(){
    return{
    energyMix:[]
  }},
  mounted(){
    fetch("https://api.carbonintensity.org.uk/generation")
    .then(response => response.json())
    .then(data => this.energyMix = this.formatEnergyMix(data))
  },
  methods: {
    formatEnergyMix(data){
      const mixes = data.data.generationmix;
      const energyMix = mixes.map(mix => {
        return [mix.fuel , mix.perc]

      })
      energyMix.unshift(["Fuel", "Percentage"])
      return energyMix
    }
  },
  components:{
    "energy-chart": EnergyChart
  }
}

</script>

<style>
*{
  text-align: center;
}
</style>
