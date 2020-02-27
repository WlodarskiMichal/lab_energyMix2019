<template>
  <div id="app">
    <h1>Energy Mix For United Kingdom</h1>
  </div>
</template>

<script>


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
      console.log(energyMix);
    }
  }}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
