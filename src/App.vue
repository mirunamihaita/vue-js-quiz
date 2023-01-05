<script setup>
import QuizApp from './components/QuizApp.vue'
</script>

<style lang="scss">
@import "./scss/main.scss";
</style>

<template>
  <div class="parallax">
    <div class="text-container">
      <h1>VUE JS QUIZ</h1>
      <p>Scroll down to begin quiz</p>
    </div>
  </div>

  <div class="quiz-container">
    <QuizApp />
  </div>

  <div class="parallax"></div>



</template>
