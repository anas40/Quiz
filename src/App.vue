<template>
  <div id="app">
    <Header :totalQuestions="totalQuestions" :numCorrect="numCorrect" :numTotal="numTotal" />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="12" md="10" lg="6" offset-md="1" offset-lg="3">
          <!-- without this v-if one error was coming, the response from server was taking time and components were mounted before that,
          the application was working as until human interaction there were questions -->
          <quiz @showFinalScore="finalScore" v-show="!runFinalScore" :totalQuestions="totalQuestions" @clicked="next" :increment="increment" v-if="questions.length" :currentQuestion="questions[index]" :index="index"/>
          <FinalScore @replay="replay" v-if="runFinalScore" :totalQuestions="totalQuestions" :numCorrect="numCorrect"/>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import FinalScore from "@/components/FinalScore.vue"
import Header from "./components/header.vue";
import quiz from "./components/Quiz.vue";

export default {
  name: "app",
  data() {
    return {
      questions: [],
      index:0,
      numCorrect:0,
      numTotal:0,
      runFinalScore:false
    };
  },
  computed:{
    totalQuestions(){
      return this.questions.length;
    }
  },
  methods:{
    replay(){
      this.questions= []
      this.index=0
      this.numCorrect=0
      this.numTotal=0
      this.runFinalScore=false
      this.fetchQuestions()
    },
    finalScore(){
      this.runFinalScore= true;
    },
    next(){
      this.index++
    },
    increment(isCorrect){
      if(isCorrect){
        this.numCorrect++
      }
      this.numTotal++
    },
    fetchQuestions() {
      fetch("https://opentdb.com/api.php?amount=5&difficulty=easy&type=multiple", {
        method: "get"
      })
      .then(response => {
        return response.json();
      })
      .then(jsondata => {
        this.questions = jsondata.results;
      })
      .catch( ()=>{
        prompt("Check internet connection")
      })
    }
  },
  components: {
    Header,
    quiz,
    FinalScore
  },
  mounted(){
    this.fetchQuestions()
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}
</style>
