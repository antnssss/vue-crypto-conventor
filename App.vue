<template>
  <h1>CRYPTO</h1>
  <Input :changeAmount="changeAmount" :convert="convert"/>
  <p v-if="error != ''">{{ error }}</p>
  <p v-if="result != 0" className="result-text">{{ result }}</p>

  <div class="selectors">
    <Selector :setCrypto="setCryptoFirst" />
    <Selector :setCrypto="setCryptoSecond"/>
  </div>
</template>

<script>
import Input from './components/Input.vue'
import Selector from './components/Selector.vue'
import CryptoConvert from 'crypto-convert';

const convert = new CryptoConvert();

export default {
  components: { Input, Selector },
  data() {
    return {
      amount: 0,
      cryptoFirst: '',
      cryptoSecond: '',
      error: '',
      result: 0,
    }
  },
  methods: {
    changeAmount(val) {
      this.amount = val;
    },
    setCryptoFirst(val) {
      this.cryptoFirst = val;
    },
    setCryptoSecond(val) {
      this.cryptoSecond = val;
    },
    async convert() {
      if (this.amount <= 0) {
        this.error = 'Enter a number bigger than 0';
        return;
      } else if (this.cryptoFirst === '' || this.cryptoSecond === '') {
        this.error = 'Choose pair';
        return;
       }else if (this.cryptoFirst === this.cryptoSecond) {
        this.error = 'Choose another pair';
        return;
       }
      

      this.error = '';

      await convert.ready();

       if(this.cryptoFirst == 'BTC' && this.cryptoSecond == 'ETH')
      this.result = convert.BTC.ETH(this.amount);
       else if(this.cryptoFirst == 'BTC' && this.cryptoSecond == 'USDT')
       this.result = convert.BTC.USDT(this.amount);
       else if(this.cryptoFirst == 'ETH' && this.cryptoSecond == 'BTC')
       this.result = convert.ETH.BTC(this.amount);
       else if(this.cryptoFirst == 'ETH' && this.cryptoSecond == 'USDT')
       this.result = convert.ETH.USDT(this.amount);
       else if(this.cryptoFirst == 'USDT' && this.cryptoSecond == 'BTC')
       this.result = convert.USDT.BTC(this.amount);
       else if(this.cryptoFirst == 'USDT' && this.cryptoSecond == 'ETH')
       this.result = convert.USDT.ETH(this.amount);
    }
  }
}
</script>

<style scoped>
.selectors {
  display: flex;
  justify-content: space-around;
  width: 700px;
  margin: 0 auto;
}

input {
  width: 500px;
  position: relative;
  top: -50px;
  font-size: 1.25em;
  outline: none;
  border-radius: 3px;
  border: 0;
  padding: 10px 15px;
  background: #fafafa;
  color: #333;
}

button {
  position: relative;
  top: -20px;
  padding: 15px 20px;
  color: #fff;
  text-transform: uppercase;
  cursor: pointer;
  background: #1a032d;
  border: 0;
  border-radius: 3px;
}
</style>
