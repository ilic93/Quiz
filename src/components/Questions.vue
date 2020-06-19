<template>
  <div>
    <b-jumbotron>
      <template v-slot:lead>
        {{ question }}
      </template>
  
      <hr class="my-4">

      <b-list-group>
        <b-list-group-item v-for="(answer, index) in answers" :key="index" @click="selectAnswer(index)" :class="answerClass(index)">
          {{ answer }}
          </b-list-group-item>
      </b-list-group>
    
      <b-button variant="primary" @click="submitAnswer" :disabled="selectedAnswer == null || answered">Submit</b-button>
      <b-button variant="success" @click="nextQuestion" :disabled="index == questions.length-1 || answered == false">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    currentQuestion: Object,
    nextQuestion: Function,
    increment: Function,
    index: Number,
    questions: Array
  },
  data() {
    return {
      selectedAnswer: null,
      correctAnswer: null,
      answered: false
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      answers = this.shuffle(answers)
      this.getCorrectIndex(answers, this.currentQuestion.correct_answer)
      return answers.map(x => this.fixWord(x))
    },
    question() {
      let question = this.fixWord(this.currentQuestion.question)
      return question
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedAnswer = index
    },
    shuffle(array) {
      var currentIndex = array.length, temporaryValue, randomIndex;
    
      // While there remain elements to shuffle...
      while (0 !== currentIndex) {
    
        // Pick a remaining element...
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex -= 1;
    
        // And swap it with the current element.
        temporaryValue = array[currentIndex];
        array[currentIndex] = array[randomIndex];
        array[randomIndex] = temporaryValue;
      }
    
      return array;
    },
    getCorrectIndex(array, x) {
      this.correctAnswer = array.indexOf(x)
    },
    submitAnswer() {
      let isCorrect = false
      if(this.selectedAnswer == this.correctAnswer) {
        isCorrect = true
      }

      this.increment(isCorrect)

      this.answered = true
    },
    answerClass(index) {
      let answerClass = (!this.answered && index == this.selectedAnswer) ? 'selected' : 
        (this.answered && index == this.correctAnswer) ? 'correct' : 
        (this.answered && this.selectedAnswer != this.correctAnswer && index == this.selectedAnswer) ? 'incorrect' : ''
      return answerClass
    },
    fixWord(x) {
      return x.replace(/&quot;/g, '"')
        .replace(/&#039;/g, "'")
        .replace(/&amp;/g, "&")
        .replace(/&ndash;/g, "-")
        .replace(/&Uuml;/g,'Ü')
    }
  },
  watch: {
    currentQuestion() {
      this.selectedAnswer = null
      this.answered = false
    }
  }
}
</script>

<style scoped>
  .list-group {
    margin-bottom: 10px;
  }
  .btn {
    margin: 10px;
  }
  .list-group-item:hover {
    cursor: pointer;
    background-color: grey;
    color: white;
    box-shadow: 5px 0 rgb(117, 64, 64);
  }

  .selected {
    background-color:chartreuse;
  }

  .correct { 
    background-color: lightgreen;
  }

  .incorrect{ 
    background-color: red;
    color: white;
  }
</style>