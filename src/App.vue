<template>
  <div id="app">
    <Header :numCorrect="numCorrect" :numTotal="numTotal" />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <!-- without this v-if one error was coming, the response from server was taking time and components were mounted before that,
          the application was working as until human interaction there were questions -->
          <quiz :increment="increment" v-if="questions.length" :currentQuestion="questions[index]" :next="next"/>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/header";
import quiz from "./components/Quiz.vue";

export default {
  name: "app",
  data() {
    return {
      questions: [],
      index:0,
      numCorrect:0,
      numTotal:0
    };
  },
  methods:{
    next(){
      this.index++;
    },
    increment(isCorrect){
      if(isCorrect){
        this.numCorrect++
      }
      this.numTotal++
    }
  },
  components: {
    Header,
    quiz
  },
  mounted() {
    fetch("https://opentdb.com/api.php?amount=10&category=9&type=multiple", {
      method: "get"
    })
      .then(response => {
        return response.json();
      })
      .then(jsondata => {
        this.questions = jsondata.results;
      })
      .catch((er)=>{
        console.log("Cant fetch the data:",er)
        prompt("Check internet connection")
        })
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
  margin-top: 60px;
}
</style>
