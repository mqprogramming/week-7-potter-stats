<template>
  <div>
    <GChart
      type="PieChart"
      :data="chartData"
      :options="chartOptions"
      style="width: 80vw; height: 80vh; margin: auto;"
    />
  </div>
</template>

<script>
  import { GChart } from 'vue-google-charts'

  export default {
    name: 'google-chart',
    data () {
      return {
        // Array will be automatically processed with visualization.arrayToDataTable function
        chartData: [
          ['Category', 'Amount'],
          ['Category1', 3000],
          ['2015', 1170],
          ['2016', 660],
          ['2017', 1030]
        ],
        chartOptions: {
          is3D: true
        }
      }
    },
    components: {
      GChart
    }
  }
</script>

<style scoped>

</style>