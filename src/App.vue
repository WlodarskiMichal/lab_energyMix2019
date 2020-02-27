<template>
  <div id="app">
    <h1>Energy Mix For United Kingdom</h1>
    <energy-chart :energyMix="energyMix"></energy-chart>
    <solar-plot :solarPlot="solarPlot"></solar-plot>
  </div>
</template>

<script>

import EnergyChart from './components/EnergyChart.vue'
import SolarPlot from './components/SolarPlot.vue'

export default {
  name: 'app',
  data(){
    return{
    energyMix:[],
    energyMix2019: [],
    solarPlot: []
  }},
  mounted(){
    // Fetch data from last 30mins
    fetch("https://api.carbonintensity.org.uk/generation")
    .then(response => response.json())
    .then(data => this.energyMix = this.formatEnergyMix(data))
    .then(energyMix => this.addTitlesToChartData(energyMix))

    //Fetch data for 2019
    this.energyMix2019 = this.fetchEnergyMix2019()

    // 2017-08-25T12:35Z
  },
  methods: {
    fetchEnergyMix2019(){
      const urls = [
        this.fromToURL("2019-01-01T00:00Z","2019-03-01T00:00Z"),
        this.fromToURL("2019-03-01T00:00Z","2019-04-29T00:00Z"),
        this.fromToURL("2019-04-29T00:00Z","2019-06-28T00:00Z"),
        this.fromToURL("2019-06-28T00:00Z","2019-08-27T00:00Z"),
        this.fromToURL("2019-08-27T00:00Z","2019-10-26T00:00Z"),
        this.fromToURL("2019-10-26T00:00Z","2019-12-25T00:00Z"),
        this.fromToURL("2019-12-25T00:00Z","2020-01-01T00:00Z")
      ];

      Promise.all(urls.map(url =>
        fetch(url)
        .then(response => response.json())
        .then(data => data.data)
      ))
      .then(results => this.energyMix2019 = results.reduce((energyMix2019,result) =>{
        return energyMix2019.concat(result)
      },[]))
      .then(results => this.solarPlot = this.solarPlotFormat(results))

    },

    fromToURL(from, to){
      return `https://api.carbonintensity.org.uk/generation/${from}/${to}`
    },

    formatEnergyMix(data){
      const mixes = data.data.generationmix;
      const energyMix = mixes.map(mix => {
        return [mix.fuel , mix.perc]
      })
      return energyMix
    },
    addTitlesToChartData(data){
      data.unshift(["Fuel", "Percentage"])
    },

    solarPlotFormat(data){
      const plotData = data.map(datum => {
        return [datum.from , datum.generationmix[8].perc]
      })
      plotData.unshift(["Time", "Solar percentage"])
      return plotData
    }
  },
  components:{
    "energy-chart": EnergyChart,
    "solar-plot": SolarPlot
  }
}

</script>

<style>
*{
  text-align: center;
}
</style>
