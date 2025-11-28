<template>
  <div class="page-wrapper">

    <!-- HEADER -->
    <header class="header">
      <h1 class="logo">Dinar Crypto Dashboard</h1>
    </header>

    <!-- MAIN CONTENT -->
    <main class="container">
      <h2 class="title">Crypto Price List</h2>

      <button @click="fetchData" class="btn-refresh">
        Refresh Data
      </button>

      <!-- Loading -->
      <div v-if="loading" class="loading">Loading...</div>

      <!-- Content -->
      <div v-else>
        <div class="grid">
          <div 
            v-for="coin in paginatedData" 
            :key="coin.id" 
            class="card"
          >
            <p><strong>Rank:</strong> {{ coin.rank }}</p>
            <p><strong>Name:</strong> {{ coin.name }} ({{ coin.symbol }})</p>
            <p><strong>Price:</strong> ${{ coin.price_usd }}</p>
          </div>
        </div>

        <!-- Pagination -->
        <div class="pagination">
          <button 
            class="page-btn" 
            :disabled="page === 1"
            @click="page--"
          >
            Prev
          </button>

          <span class="page-number">Page {{ page }} / {{ totalPages }}</span>

          <button 
            class="page-btn" 
            :disabled="page === totalPages"
            @click="page++"
          >
            Next
          </button>
        </div>
      </div>
    </main>

    <!-- FOOTER -->
    <footer class="footer">
      <p>© 2025 Dinar Crypto App — All Rights Reserved</p>
    </footer>

  </div>
</template>

<script>
export default {
  data() {
    return {
      coins: [],
      loading: false,
      page: 1,
      perPage: 4  // default mobile
    };
  },

  computed: {
    totalPages() {
      return Math.ceil(this.coins.length / this.perPage);
    },

    paginatedData() {
      const start = (this.page - 1) * this.perPage;
      return this.coins.slice(start, start + this.perPage);
    }
  },

  methods: {
    async fetchData() {
      this.loading = true;
      try {
        const response = await fetch("https://api.coinlore.net/api/tickers/");
        const result = await response.json();
        this.coins = result.data;
      } catch (error) {
        console.error("Gagal fetch data:", error);
      } finally {
        this.loading = false;
      }
    },

    setPerPageByDevice() {
      if (window.innerWidth >= 1024) {
        this.perPage = 14; // desktop
      } else {
        this.perPage = 2; // mobile/tablet
      }
    }
  },

  mounted() {
    this.setPerPageByDevice();
    this.fetchData();

    window.addEventListener("resize", this.setPerPageByDevice);
  },

  beforeUnmount() {
    window.removeEventListener("resize", this.setPerPageByDevice);
  }
};
</script>