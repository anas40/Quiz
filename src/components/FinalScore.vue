<template>
  <div>
    <b-jumbotron>
        <div>Your Final Score was: {{numCorrect}}/{{totalQuestions}}</div>
    </b-jumbotron>
    <b-button @click="replay" variant="primary">Replay</b-button>
  </div>
</template>

<script>
export default {
    props:{
        numCorrect:Number,
        totalQuestions:Number
    },
    methods:{
        replay(){
            this.$emit('replay')
        }
    }
};
</script>

<style scoped>
.jumbotron{
    margin: 30px 0;
}
.jumbotron>div{
    font-size:24px;
    color:black;
}
</style>