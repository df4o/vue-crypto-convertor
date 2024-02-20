<script>
import MyInput from './components/MyInput.vue'
import MySelector from './components/MySelector.vue'
import CryptoConvert from 'crypto-convert'
import MyFavorite from './components/MyFavorite.vue'

const cryptoConvert = new CryptoConvert()

export default {
  components: { MyInput, MySelector, MyFavorite },
  data() {
    return {
      amount: 0,
      cryptoFirst: '',
      cryptoSecond: '',
      error: '',
      result: 0,
      favs: []
    }
  },
  methods: {
    getFromFavs(index) {
      this.cryptoFirst = this.favs[index].from;
      this.cryptoSecond = this.favs[index].to;
    },
    favorite() {
      this.favs.push({
        from: this.cryptoFirst,
        to: this.cryptoSecond
      })
    },
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
      if(this.amount <= 0) {
        this.error = 'Enter number > 0';
        return;
      } else if(this.cryptoFirst === this.cryptoSecond) {
        this.error = 'Change another currency';
        return;
      } else if(this.cryptoFirst === '' || this.cryptoSecond === '') {
        this.error = 'Empty currency!';
        return;
      }

      this.error = '';

      await cryptoConvert.ready();

      this.result = cryptoConvert[this.cryptoFirst][this.cryptoSecond](this.amount);
      
    }
  }
}
</script>

<template>
  <h1>CRYPTO</h1>
  <MyInput :changeAmount="changeAmount" :convert="convert"/>
  <div>
    <button @click="convert()">Convert</button>
    <button @click="favorite()" className="fav-btn">Favorite</button>
  </div>
  <p className="error" v-if="error !== ''">{{ error }}</p>
  <p className="text-result" v-if="result !== 0">{{ result }}</p>
  <MyFavorite :favs="favs" :getFromFavs="getFromFavs" v-if="favs?.length > 0"/>
  <div className="selectors">
    <MySelector :setCrypto="setCryptoFirst" :cryptoNow="cryptoFirst"/>
    <MySelector :setCrypto="setCryptoSecond" :cryptoNow="cryptoSecond"/>
  </div>
</template>
 
<style scoped>
.selectors {
  display: flex;
  justify-content: space-around;
  width: 700px;
  margin: 0 auto;
}
button {
  position: relative;
  top: -20px;
  padding: 15px 20px;
  color: #fff;
  margin: 0 auto;
  text-transform: uppercase;
  cursor: pointer;
  background: #1a032d;
  border: 0;
  border-radius: 3px;
}
.text-result {
  font-family: cursive;
  font-size: 2em;
  color: lightgray;
}
.error {
  color: red;
}
.fav-btn {
  margin-left: 20px;
}
</style>
