<template>
  <div id="app">
    <h1>Potter Stats</h1>
    <p>
      Show me <broad-subject-selector :subjects="subjects"></broad-subject-selector> for <subject-selector :subjects="subjects" v-on:change="createNewData()"></subject-selector>
    </p>
    <google-chart :newDataArray="newDataArray"></google-chart>
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
          label: "Species",
          rawLabel: "species",
          options: ["", "Humans", "Werewolves", "Half Giants", "Part Goblins", "Ghosts", "House Elves", "Goblins", "Giants", "Centaurs", "Part Humans"],
          rawOptions: ["", "human", "werewolf", "half-giant", "part-goblin", "ghost", "house-elf", "goblin", "giant", "centaur", "part-human"]
        },
        {
          label: "Blood Status",
          rawLabel: "bloodStatus",
          options: ["", "Pure Bloods", "Half Bloods", "Muggle Borns", "Squibs", "Muggles", "Unknown"],
          rawOptions: ["", "pure-blood", "half-blood", "muggle-born", "squib", "muggle", "unknown"]
        },
        {
          label: "Ministry of Magic Employees",
          rawLabel: "ministryOfMagic",
          options: ["", "MoM Employees", "MoM Non-Employees"],
          rawOptions: ["", true, false]
        },
        {
          label: "Order of the Phoenix Members",
          rawLabel: "orderOfThePhoenix",
          options: ["", "OotP Members", "OotP Non-Members"],
          rawOptions: ["", true, false]
        },
        {
          label: "Dumbledore's Army Members",
          rawLabel: "dumbledoresArmy",
          options: ["", "DA Members", "DA Non-Members"],
          rawOptions: ["", true, false]
        },
        {
          label: "Death Eaters",
          rawLabel: "deathEater",
          options: ["", "Followers", "Non-Followers"],
          rawOptions: ["", true, false]
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
      this.newDataObject = {};
      this.newDataArray = [];

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
body {
  background-image: url("https://c.wallhere.com/photos/34/19/Harry_Potter_Hogwarts-1476055.jpg!d");
  background-repeat: no-repeat;
  background-attachment: fixed;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #4d80b3;
  margin-top: 60px;
}
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #4d80b3;
  margin-top: 60px;
} */
</style>
