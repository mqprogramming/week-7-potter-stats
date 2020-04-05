<template>
  <div id="app">
    <h1>Test</h1>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      potterData: []
    };
  },
  mounted(){
    fetch(`https://www.potterapi.com/v1/characters?deathEater=true&key=${process.env.VUE_APP_API_KEY}`)
    .then(res => res.json() )
    .then(data => this.potterData = data)
    .catch(err => console.log(err))
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
