<template>
  <div>
    <form>
      <select v-model="selectedSubject" v-if="!selectedOption">
        <option v-for="(subject, index) in subjects" :value="subject" :key="index">{{ subject.label }}</option>
      </select>
      <select v-model="selectedOption" v-if="selectedSubject != 'no-value'" v-on:change="handleChange()">
        <option v-for="(option, index) in selectedSubject.options" :value="selectedSubject.rawOptions[index]" :key="index">{{ option }}</option>
      </select>
    </form>
  </div>
</template>

<script>
  import { eventBus } from '../main.js';

  export default {
    name: 'subject-selector',
    props: ['subjects'],
    data(){
      return {
        selectedSubject: 'no-value',
        selectedOption: '',
        subjectOptionPair: {}
      }
    },
    methods: {
      handleChange() {
        this.subjectOptionPair = {};
        this.subjectOptionPair[this.selectedSubject.rawLabel] = this.selectedOption;
        eventBus.$emit('subject-change', this.subjectOptionPair)
      }
    }
  }
</script>

<style scoped>

</style>