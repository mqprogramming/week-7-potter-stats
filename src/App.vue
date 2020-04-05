<template>
  <div id="app">
    <h1>Potter Stats</h1>
    <p>
      Show me <broad-subject-selector :subjects="subjects"></broad-subject-selector> for <subject-selector :subjects="subjects" v-on:change="createNewData()"></subject-selector>
    </p>
    <google-chart></google-chart>
  </div>
</template>

<script>
import SubjectSelector from './components/SubjectSelector.vue';
import BroadSubjectSelector from './components/BroadSubjectSelector.vue';
import GoogleChart from './components/GoogleChart.vue';
import { eventBus } from './main.js';

export default {
  name: 'App',
  data() {
    return {
      potterData: [],
      subjects: [
        {
          label: "Houses",
          rawLabel: "house",
          options: ["", "Gryffindors", "Hufflepuffs", "Ravenclaws", "Slytherins"],
          rawOptions: ["", "Gryffindor", "Hufflepuff", "Ravenclaw", "Slytherin"]
        },
        {
          label: "Blood Status",
          rawLabel: "bloodStatus",
          options: ["", "Pure Bloods", "Half Bloods", "Muggle Borns", "Muggles", "Unknown"],
          rawOptions: ["", "pure-blood", "half-blood", "muggle-born", "muggles", "unknown"]
        }
      ],
      selectedBroadSubject: "",
      selectedSubOptPair: {},
      newDataObject: {},
      newDataArray: []
    };
  },
  mounted(){
    fetch(`https://www.potterapi.com/v1/characters?key=${process.env.VUE_APP_API_KEY}`)
    .then(res => res.json() )
    .then(data => this.potterData = data)
    .catch(err => console.log(err))

    eventBus.$on('broad-subject-change', (subject) => {
      this.selectedBroadSubject = subject
    })

    eventBus.$on('subject-change', (subject) => {
      this.selectedSubOptPair = subject
    })
  },
  watch: {
    selectedSubOptPair: function (newSubOptPair, oldSubOptPair) {
      // Creates object with rawLabels keys and values of 0
      this.subjects.forEach(subject => {
        if(this.selectedBroadSubject == subject.rawLabel){
          subject.rawOptions.forEach(value => {
            if(value != ""){
              this.newDataObject[value] = 0
            }
          })
        };
      });

      // Filters characters based on specific requirement
      let property = Object.keys(this.selectedSubOptPair)[0];
      let filteredData = this.potterData.filter(character => 
        (character[property] == this.selectedSubOptPair[property])
      );

      // Checks if character property is in object and counts total numbers
      Object.keys(this.newDataObject).forEach(key => {
        filteredData.forEach((character) => {
            if(character[this.selectedBroadSubject] == key){
              this.newDataObject[key]++;
            }
          }
        )
      });
      
      // Converts object into nested arrays
      this.newDataArray = Object.entries(this.newDataObject);

     }
  },
  methods: {

  },
  components: {
    'subject-selector': SubjectSelector,
    'broad-subject-selector': BroadSubjectSelector,
    'google-chart': GoogleChart
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
