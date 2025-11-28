<template>
  <div style="max-width: 450px; margin: auto; padding: 20px; font-family: Arial;">
    <h2 style="text-align:center;">Crypto Price List</h2>

    <button 
      @click="fetchData" 
      style="width: 100%; padding: 10px; margin-bottom: 15px; cursor:pointer;"
    >
      Refresh Data
    </button>

    <div v-if="loading">Loading...</div>

    <div v-else>
      <div 
        v-for="coin in coins" 
        :key="coin.id" 
        style="border:1px solid #ccc; padding: 10px; margin-bottom: 10px; border-radius: 8px;"
      >
        <p><strong>Rank:</strong> {{ coin.rank }}</p>
        <p><strong>Name:</strong> {{ coin.name }} ({{ coin.symbol }})</p>
        <p><strong>Price USD:</strong> ${{ coin.price_usd }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      coins: [],
      loading: false
    };
  },

  methods: {
    async fetchData() {
      this.loading = true;
      try {
        const response = await fetch("https://api.coinlore.net/api/tickers/");
        const result = await response.json();
        this.coins = result.data;
      } catch (error) {
        console.error("Gagal mengambil data:", error);
      } finally {
        this.loading = false;
      }
    }
  },

  mounted() {
    this.fetchData();
  }
};
</script>
