<template>
  <div>
    <button @click="updateData">Show Chart</button>
    <GChart id="chart"
      type="PieChart"
      :data="chartData"
      :options="chartOptions"
    />
  </div>
</template>

<script>
  import { GChart } from 'vue-google-charts'

  export default {
    name: 'google-chart',
    props: ['newDataArray'],
    data () {
      return {
        chartDataHeader: ['Foo', 'Bar'],
        updatedChartData: [],
        chartOptions: {
            backgroundColor: {
              fill: 'transparent'
            },
            legend: {
              textStyle: {
                color: 'white'
              }
            },
            is3D: true
          }
        }
    },
    components: {
      GChart
    },
    computed: {
      chartData () {
        return [ this.chartDataHeader, ...this.updatedChartData ]
      }
    },
    methods: {
      updateData () {
        this.updatedChartData = this.newDataArray
      }
    }
    // watch: {
    //   newDataArray: function (newStuff, oldStuff) {
    //     updateData();
    //   }
    // }
  };
</script>

<style scoped>
  #chart {
    width: 60vw;
    height: 60vh; 
    margin: auto; 
    margin-top: 20px;
  }
</style>