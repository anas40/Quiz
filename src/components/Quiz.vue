<template>
  <div>
    <b-jumbotron>
      <template v-slot:lead>{{currentQuestion.question}}</template>

      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item
          v-for="(answer,index) in answers"
          :key="answer"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
        >
        {{answer}}
        </b-list-group-item>
      </b-list-group>

      <b-button 
      @click="submitAnswer" 
      variant="primary"
      :disabled="selectedIndex === null || answered" 
      >
      Submit
      </b-button>
      <b-button @click="next" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    isCorrect:Function,
    increment:Function
  },
  watch: {
    currentQuestion: {//watch runs when props change only
      immediate: true,//with this we can run when props passed as well on change
      handler() {
        this.shuffleAnswer();
        this.answered = false
      }
    }
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex:null,
      shuffledAnswers: [],
      answered:false
    };
  },
  computed: {
    answers() {
      let a = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      return a;
    }
  },
  methods: {
    selectAnswer(index) {
      console.log("yes target selectAnswer")
      this.selectedIndex = index;
    },
    submitAnswer(){
        let isCorrect = false;
        if(this.selectedIndex === this.correctIndex){
            isCorrect = true
        }
        this.answered = true
        this.increment(isCorrect)
    },
    shuffleAnswer() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    answerClass(index){
        let answerClass = ''
        if(this.selectedIndex === index && !this.answered){
            answerClass = "selected"
        }
        else{
            if(index === this.correctIndex && this.answered){
                answerClass = "correct"
            }
            if(this.selectedIndex === index && index !== this.correctIndex && this.answered){
                answerClass = "incorrect"
            }
        }
        return answerClass
    }
  }
};
</script>
<style scoped>
.list-group-item {
  margin-bottom: 15px;
}
.btn {
  margin: 0 5px;
}
.list-group-item:hover {
  cursor: pointer;
  background-color: #eee;
}
.selected {
  background-color: lightblue;
}
.correct {
  background-color: lightgreen;
}
.incorrect {
  background-color: red;
}
</style>