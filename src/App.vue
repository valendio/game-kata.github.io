<template>
  <div id="app">
    <Game v-if="gameActive" @game-stopped="handleGameStop" />
    <Stats 
      v-else
      :stats="stats"
      :recentWords="recentWords"
      @restart-game="restartGame"
    />
  </div>
</template>

<script>
import Game from './components/GameWeb.vue'
import Stats from './components/StatsWeb.vue'

export default {
  components: { Game, Stats },
  data() {
    return {
      gameActive: true,
      stats: { correct: 0, incorrect: 0 },
      recentWords: []
    }
  },
  methods: {
    handleGameStop(stats, words) {
      this.stats = stats
      this.recentWords = words
      this.gameActive = false
    },
    restartGame() {
      this.gameActive = true
      this.stats = { correct: 0, incorrect: 0 }
      this.recentWords = []
    }
  }
}
</script>

<style>
/* Global Styles */
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}

#app {
  min-height: 100vh;
  background: #E8BFE8;
  padding: 2rem;
}
</style>