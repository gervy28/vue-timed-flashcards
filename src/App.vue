<template>
  <div id="app" class="lg:mx-56 md:mx-32">
    <countdown></countdown>
    <question-card></question-card>
  </div>
</template>

<script>
import Countdown from './components/countdown.vue'
import QuestionCard from './components/question-card.vue'

export default {
  name: 'App',
  components: {
    Countdown,
    QuestionCard
  }
}
</script>

<style>
#app{
  @apply flex; 
  @apply flex-col; 
  @apply text-center; 
}
</style>
