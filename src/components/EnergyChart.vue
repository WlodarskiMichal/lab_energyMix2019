<template lang="html">
<GChart class="PieChart"
type="PieChart"
:data="energyMix"
:options="chartOptions"/>
<!-- <p>{{energyMix}}</p> -->
</template>


<script>
import {GChart} from 'vue-google-charts'
export default {
  name:"chart",
  components: {
    GChart
  },
  props:['energyMix'],
  data(){
    return{
      chartOptions:{
        title: 'Energy Mix For United Kingdom'
      }
    }
  }
}
</script>

<style lang="css" scoped>
.PieChart{
width:1100px;
height:1000px;
}
</style>
