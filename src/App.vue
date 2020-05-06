<template>
  <div id="app">
    <RedDotTest v-on:test-result="onTestResult" :show-for-ms="2000"/>
  </div>
</template>

<script>
import RedDotTest from './components/RedDotTest.vue'

export default {
  name: 'App',
  components: {
    RedDotTest
  },
  methods:{
    onTestResult(value){
      console.log("parent:" + value)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
