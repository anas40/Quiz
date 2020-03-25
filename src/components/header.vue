<template>
  <div>
    <b-nav tabs>
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
</style>