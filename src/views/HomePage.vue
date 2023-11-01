<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title class="title">Cryptocurrency List</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content>
      <ion-list>
        <ion-item v-for="crypto in cryptocurrencies" :key="crypto.id" class="crypto-item">
          <ion-label class="crypto-label">
            <h2 class="crypto-name">{{ crypto.name }}</h2>
            <p class="crypto-symbol">{{ crypto.symbol }}</p>
            <p class="crypto-price">${{ crypto.price_usd }}</p>
          </ion-label>
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      cryptocurrencies: []
    };
  },
  mounted() {
    this.fetchCryptos();
  },
  methods: {
    async fetchCryptos() {
      try {
        const response = await axios.get('https://api.coinlore.net/api/tickers/', {
          params: {
            vs_currency: 'usd',
            order: 'market_cap_desc',
            per_page: 10,
            page: 1,
            sparkline: false
          }
        });
        this.cryptocurrencies = response.data.data;
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    }
  }
};
</script>

<style scoped>
.title {
  font-size: 24px;
  font-weight: bold;
  text-align: center;
  margin-top: 20px;
}

.crypto-item {
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-bottom: 10px;
  background-color: #f5f5f5; 
}

.crypto-label {
  padding: 10px;
}

.crypto-name {
  font-size: 20px;
  font-weight: bold;
}

.crypto-symbol {
  font-size: 16px;
  color: #333;
}

.crypto-price {
  font-size: 16px;
  color: #007bff; 
}
</style>
