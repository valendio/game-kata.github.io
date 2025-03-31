<template>
  <div class="stats-container">
    <div class="stats-box">
      <h2>Statistik Anda</h2>
      <div class="stats-grid">
        <div class="stat-item">
          <div class="stat-value">{{ stats.correct }}</div>
          <div class="stat-label">Benar</div>
        </div>
        <div class="stat-item">
          <div class="stat-value">{{ stats.incorrect }}</div>
          <div class="stat-label">Salah</div>
        </div>
        <div class="stat-item">
          <div class="stat-value">{{ accuracy }}%</div>
          <div class="stat-label">Akurasi</div>
        </div>
      </div>
    </div>

    <div class="word-list">
      <h3>Kata-Kata Hari Ini</h3>
      <table>
        <thead>
          <tr>
            <th>Kata Baku</th>
            <th>Kata Tidak Baku</th>
            <th>Pengertian</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(word, index) in recentWords" :key="index">
            <td>{{ word.baku }}</td>
            <td>{{ word["tak-baku"] }}</td>
            <td>{{ word.pengertian }}</td>
          </tr>
        </tbody>
      </table>
    </div>

    <button @click="$emit('restart-game')" class="restart-btn">
      Main Lagi
    </button>
  </div>
</template>

<script>
export default {
  props: {
    stats: Object,
    recentWords: Array,
  },
  computed: {
    accuracy() {
      const total = this.stats.correct + this.stats.incorrect;
      return total > 0 ? Math.round((this.stats.correct / total) * 100) : 0;
    },
  },
};
</script>

<style scoped>
.stats-container {
  max-width: 800px;
  margin: 0 auto;
}

.stats-box {
  background: white;
  border-radius: 10px;
  padding: 2rem;
  margin-bottom: 2rem;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 2rem;
  margin-top: 1.5rem;
}

.stat-item {
  text-align: center;
}

.stat-value {
  font-size: 2.5rem;
  font-weight: bold;
  color: #e8bfe8;
}

.stat-label {
  color: #666;
}

.word-list {
  background: white;
  border-radius: 10px;
  padding: 2rem;
  margin-bottom: 2rem;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 1rem;
}

th,
td {
  padding: 0.8rem;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

th {
  background-color: #f8f8f8;
}

.restart-btn {
  padding: 1rem 2rem;
  background: #e8bfe8;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  display: block;
  margin: 0 auto;
}
</style>
