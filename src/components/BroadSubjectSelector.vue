<template>
  <div>
    <form>
      <select v-model="selectedBroadSubject" v-on:change="handleChange()">
        <option v-for="(subject, index) in subjects" :value="subject.rawLabel" :key="index">{{ subject.label }}</option>
      </select>
    </form>
  </div>
</template>

<script>
  import { eventBus } from '../main.js';

  export default {
    name: 'broad-subject-selector',
    props: ['subjects'],
    data(){
      return {
        selectedBroadSubject: "no-value"
      }
    },
    methods: {
      handleChange() {
        eventBus.$emit('broad-subject-change', this.selectedBroadSubject)
      }
    }
  }
</script>

<style scoped>

</style>