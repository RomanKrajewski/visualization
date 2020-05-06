<template>
  <div id="app">
    <RedDotTest ref="redDotTest" v-on:test-result="onTestResult" v-on:start-test="startTest" :example-stage="true" />
  </div>
</template>

<script>
import RedDotTest from './components/RedDotTest.vue'

export default {
  name: 'App',
  components: {
    RedDotTest
  },
  data: function () {
    return {
      currentTestIndex: 0,
      nTestsPerTime: 2,
      testedTimes: [2000, 1000, 500, 250, 125]
    };
  },
  methods:{
    onTestResult(result){
      if(result === "correct"){
        const nTotalTests = this.nTestsPerTime * this.testedTimes.length
        this.currentTestIndex ++;
        if(this.currentTestIndex < nTotalTests){
          this.startTest(this.currentTestIndex)
        }
      }
    },
    startTest(testIndex){
      this.currentTestIndex = testIndex;
      this.$refs.redDotTest.exampleStage = false;
      this.$refs.redDotTest.startTest(this.testedTimes[Math.floor(this.currentTestIndex/this.nTestsPerTime)]);
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
