<template>
  <div>
    
    <b-nav tabs>
    <img id="logo" :src="require('@/assets/quiz.svg')" style="{display:inline;}">
      <b-nav-item disabled><b>Quiz</b></b-nav-item>
      <b-nav-item disabled>Answered: {{numTotal}}</b-nav-item>
      <b-nav-item disabled>Correct: {{numCorrect}}</b-nav-item>
      <b-nav-item disabled>Total Questions: {{totalQuestions}}</b-nav-item>
    </b-nav>
    
  </div>
</template>

<script>
export default {
  props:{
    numCorrect:Number,
    numTotal:Number,
    totalQuestions:Number
  }
}
</script>
<style scoped>
ul h2{
  align-self:flex-end;
}
#logo{
  margin-left:12px;
}
</style>