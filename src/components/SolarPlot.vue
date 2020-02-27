<template lang="html">
<GChart class="lineChart"
  type="LineChart"
  :data="solarPlot"
  :options="chartOptions"
/>
</template>


<script>
import {GChart} from 'vue-google-charts'

export default {
  name:"chart",
  components: {
    GChart
  },
  props:['solarPlot'],
  data(){
    return{
      chartOptions:{
        title: 'Percentage of solar power for UK 2019',
      }
    }
  }
}
</script>

<style lang="css" scoped>
.lineChart{
width:2100px;
height:1000px;

}
</style>
