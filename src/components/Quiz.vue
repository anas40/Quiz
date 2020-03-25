<template>
  <div>
    <b-jumbotron>
      <p id="questionNo">Q.{{index+1}}</p><div v-html="currentQuestion.question"></div>
      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item
          v-for="(answer,index) in shuffledAnswers"
          :key="answer"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
          v-html="answer"
        >
        </b-list-group-item>
      </b-list-group>

      <b-button 
      @click="submitAnswer" 
      variant="primary"
      :disabled="selectedIndex === null || answered" 
      >
      Submit
      </b-button>
      <b-button v-if="index<totalQuestions-1" @click="next" variant="success" :disabled="!answered">Next</b-button>
    </b-jumbotron>
    <div>
  
</div>

  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: {
    currentQuestion: Object,
    isCorrect:Function,
    increment:Function,
    index:Number,
    totalQuestions:Number
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
  methods: {
    next(){
      this.selectedIndex= null;
      this.$emit('clicked')
    },
    selectAnswer(index) {
      if(!this.answered){
      this.selectedIndex = index;
      }
    },
    submitAnswer(){
        let isCorrect = false;
        if(this.selectedIndex === this.correctIndex){
            isCorrect = true
        }
        this.answered = true
        this.increment(isCorrect)
        if(this.index === this.totalQuestions-1){
          setTimeout(() => {this.$emit('showFinalScore'); }, 2000);
        }
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
button.disabled{
  cursor:unset;
}

#questionNo{
  position: relative;
  top: -20px;
  font-size:26px;
  font-weight:  700;
  color: rgb(42, 42, 42);
}
.jumbotron{
  margin: 1rem 0;
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
.selected:hover{
  background-color: lightblue;
}
.correct,.correct:hover {
  background-color: rgb(169, 255, 169);
}
.incorrect,.incorrect:hover {
  background-color: rgb(255, 126, 126);
}
</style>