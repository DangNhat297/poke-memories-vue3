<template>
  <main-screen v-if="statusMatch === 'default'" @onStart="onHandleBeforeStart($event)"/>
  <interact-screen 
  v-if="statusMatch === 'match'" 
  :cardsContext="settings.cardsContext"
  @onFinish="onGetResult()"/>
  <result-screen v-if="statusMatch === 'result'" :timer="timer" @onStartAgain="statusMatch = 'default'"/>
</template>

<script>
import MainScreen from './components/MainScreen.vue';
import InteractScreen from './components/InteractScreen.vue';
import ResultScreen from './components/ResultScreen.vue';
import { shuffle } from './helper/helper';
export default {
  name: 'App',
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen
  },
  data() {
    return {
      statusMatch: "default",
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null
      },
      timer: 0
    }
  },
  methods: {
    onHandleBeforeStart(config) {
      this.settings.totalOfBlocks = config.totalOfBlock 

      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks/2 }, 
        (_, i) => i+1
      )

      const secondCards = [...firstCards]

      const finalCards = [...firstCards, ...secondCards]

      this.settings.cardsContext = shuffle(shuffle(shuffle(finalCards)))

      this.settings.startedAt = new Date().getTime()

      this.statusMatch = 'match'
    },
    onGetResult() {
      this.timer = new Date().getTime() - this.settings.startedAt
      this.statusMatch = 'result'
    },
  },
}
</script>

<style>
  
</style>