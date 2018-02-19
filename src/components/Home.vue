<template>
  <div>
    <select>
      <option v-for="s in symbols">
        {{ s.baseAsset }} / {{ s.quoteAsset }}
      </option>
    </select>
  </div>
</template>

<script>
import binanceApi from "../../node_modules/binance/lib/binance.js";
import config from "../config/DEV.config.js";

const binanceRest = new binanceApi.BinanceRest({
  key: config.api_key,
  secret: config.api_secret,
  timeout: 15000, // Optional, defaults to 15000, is the request time out in milliseconds
  recvWindow: 10000, // Optional, defaults to 5000, increase if you're getting timestamp errors
  disableBeautification: false,
  /*
     * Optional, default is false. Binance's API returns objects with lots of one letter keys.  By
     * default those keys will be replaced with more descriptive, longer ones.
     */
  handleDrift: false
  /* Optional, default is false.  If turned on, the library will attempt to handle any drift of
     * your clock on it's own.  If a request fails due to drift, it'll attempt a fix by requesting
     * binance's server time, calculating the difference with your own clock, and then reattempting
     * the request.
     */
});

export default {
  name: "Home",
  data() {
    return {
      symbols: []
    };
  },
  created() {
    binanceRest
      .exchangeInfo()
      .then(data => {
        this.symbols = data.symbols;
      })
      .catch(err => {
        console.error(err);
      });
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
