<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled><b>Quiz</b></b-nav-item>
      <b-nav-item disabled>Count: {{numCorrect}}/{{numTotal}}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props:{
    numCorrect:Number,
    numTotal:Number
  }
}
</script>