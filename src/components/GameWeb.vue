<template>
  <div class="game-container">
    <div class="header">
      <h1>Tebak Kata Baku</h1>
      <button @click="stopGame" class="stop-btn">Stop Game</button>
    </div>

    <div class="game-content">
      <div class="word-options">
        <button
          v-for="(option, index) in shuffledOptions"
          :key="index"
          @click="checkAnswer(option.isBaku)"
          class="word-btn"
        >
          {{ option.word }}
        </button>
      </div>

      <div
        class="result-box"
        :class="{ correct: isCorrect, incorrect: !isCorrect && isAnswered }"
      >
        <div v-if="isAnswered" class="result-content">
          <h3>{{ isCorrect ? "Benar!" : "Salah!" }}</h3>
          <p><strong>Pengertian:</strong> {{ currentWord.pengertian }}</p>
          <p><strong>Kata Baku:</strong> {{ currentWord.baku }}</p>
          <p><strong>Kata Tidak Baku:</strong> {{ currentWord["tak-baku"] }}</p>
          <button @click="nextWord" class="next-btn">Lanjut</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import words from "@/assets/tak-baku-vs-baku.json";

export default {
  data() {
    return {
      words: [],
      currentWord: null,
      isAnswered: false,
      isCorrect: false,
      stats: { correct: 0, incorrect: 0 },
      answeredWords: [],
    };
  },
  computed: {
    shuffledOptions() {
      if (!this.currentWord) return [];
      const options = [
        { word: this.currentWord.baku, isBaku: true },
        { word: this.currentWord["tak-baku"], isBaku: false },
      ];
      return options.sort(() => Math.random() - 0.5);
    },
  },
  mounted() {
    this.words = words;
    this.getNewWord();
  },
  methods: {
    getNewWord() {
      const randomIndex = Math.floor(Math.random() * this.words.length);
      this.currentWord = this.words[randomIndex];
      this.isAnswered = false;
    },
    checkAnswer(isBaku) {
      if (this.isAnswered) return;

      this.isAnswered = true;
      this.isCorrect = isBaku;

      if (isBaku) {
        this.stats.correct++;
      } else {
        this.stats.incorrect++;
      }

      this.answeredWords.push(this.currentWord);
    },
    nextWord() {
      if (this.answeredWords.length >= 1000) {
        this.stopGame();
        return;
      }
      this.getNewWord();
    },
    stopGame() {
      this.$emit("game-stopped", this.stats, this.answeredWords);
    },
  },
};
</script>

<style scoped>
.game-container {
  max-width: 800px;
  margin: 0 auto;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
}

.word-options {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
  margin-bottom: 2rem;
}

.word-btn {
  padding: 1.5rem;
  font-size: 1.2rem;
  border: none;
  border-radius: 10px;
  background: white;
  cursor: pointer;
  transition: transform 0.2s;
}

.word-btn:hover {
  transform: scale(1.05);
}

.result-box {
  background: white;
  border-radius: 10px;
  padding: 2rem;
  min-height: 200px;
  transition: background-color 0.3s;
}

.result-box.correct {
  background: #c8e6c9;
}

.result-box.incorrect {
  background: #ffcdd2;
}

.next-btn {
  margin-top: 1rem;
  padding: 0.5rem 2rem;
  background: #e8bfe8;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.stop-btn {
  padding: 0.5rem 1.5rem;
  background: #ffffff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
</style>
