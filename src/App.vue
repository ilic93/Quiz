<template>
  <div id="app">
    <Header :correct="right" :total="finished"/>
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <Questions
          v-if="questions.length" 
          :currentQuestion="questions[index]"
          :nextQuestion="next"
          :increment="increment"
          :index="index"
          :questions="questions"
          />
        </b-col>
      </b-row>
    </b-container>    
  </div>
</template>

<script>
import Header from './components/Header'
import Questions from './components/Questions'

export default {
  name: 'App',
  components: {
    Header,
    Questions
  },
  data() {
    return {
      questions: [],
      index: 0,
      right: 0,
      finished: 0
    }
  },
  methods: {
    next() {
      this.index++
    },
    increment(isCorrect) {
      if(isCorrect) {
        this.right++
      }
      this.finished++
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=15&category=12&type=multiple')
    .then(x => x.json())
    .then(x => this.questions = x.results)
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
  background: linear-gradient(to right, blue, red);
}
</style>
