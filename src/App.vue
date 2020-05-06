<template>
  <div id="app">
    <RedDotTest v-if="testRunning" ref="redDotTest" v-on:test-result="onTestResult" v-on:start-test="startTest" :current-test-stage="currentFeatureTest" :example-stage="exampleStage" />
    <h3 v-if="!testRunning">Fertig!</h3>
  </div>
</template>

<script>
import RedDotTest from './components/RedDotTest.vue'
export default {
  name: 'App',
  components: {
    RedDotTest,
  },
  data: function () {
    return {
      testRunning: true,
      exampleStage:true,
      currentFeatureTest: 0,
      currentTestIndex: 0,
      nTestsPerTime: 3,
      testedTimes: [2000, 1000, 500, 250, 125]
    };
  },
  computed:{
    currentlyTestedTime: function () {
      const index = Math.floor(this.currentTestIndex/this.nTestsPerTime)
      return this.testedTimes[index]
    },
    previouslyTestedTime: function(){
      const index = Math.floor(this.currentTestIndex/this.nTestsPerTime) - 1
      if(index < 0){
        return "over 2000"
      }
      return this.testedTimes[index]
    }
  },
  methods:{
    onTestResult(result){
      if(result === "wrong"){
        this.saveResult();
      }
      if(result === "correct"){
        const nTotalTests = this.nTestsPerTime * this.testedTimes.length
        if(this.currentTestIndex < nTotalTests){
          this.startTest(this.currentTestIndex)
        }
        else{
          this.saveResult()
        }
      }
    },
    startTest(testIndex){
      this.currentTestIndex = testIndex;
      this.exampleStage = false;
      this.$refs.redDotTest.startTest(this.currentlyTestedTime);
      this.currentTestIndex ++;
    },
    saveResult(){
      switch (this.currentFeatureTest) {
        case 0:
          document.getElementById("red-dot-test-result").value = this.previouslyTestedTime
          break;
        case 1:
          document.getElementById("more-red-dots-test-result").value = this.previouslyTestedTime
          break;
        case 2:
          document.getElementById("more-colors-test-result").value = this.previouslyTestedTime
          break;
        case 3:
          document.getElementById("square-test-result").value = this.previouslyTestedTime
          break;
      }
      this.currentFeatureTest ++;
      if(this.currentFeatureTest > 3){
        this.submitFinalResult()
      }else{
        this.$nextTick(() => {
          this.$refs.redDotTest.render(true);
        })
        this.exampleStage = true;
      }
    },
    submitFinalResult(){
      this.testRunning = false;
      document.forms["test-result"].submit()
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

button {
  background-color: #555555; /* Green */
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 20px;
}

</style>
