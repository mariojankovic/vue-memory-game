<template>
  <div id="app">
    <p>clicks: {{ clicks }}</p>
    <p>parovi: {{ pairs }}</p>
    <p>prvi: {{ first }}</p>
    <p>drugi: {{ second }}</p>

    <ul class="cards">
      <Card v-for="n in 12" :key="n" :n="n" @click.native="checkCard(n)"/>
    </ul>
  </div>
</template>

<script>
import Card from "./components/Card";

export default {
  name: "App",
  components: {
    Card
  },
  data() {
    return {
      clicks: 0,
      pairs: [],
      first: null,
      second: null
    };
  },
  methods: {
    checkCard(n) {
      this.clicks++;

      if (n % 2 === 0) {
        n = n - 1;
      }

      if (this.clicks % 2 === 0) {
        this.second = n;

        if (this.first === this.second) this.pairs.push(this.first);
      } else {
        this.first = n;
        this.second = null;
      }
    }
  }
};
</script>

<style>
img {
  max-width: 100%;
  height: auto;
}

.cards {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-gap: 20px;
  margin: 0;
  padding: 0;
}

.card {
  display: block;
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
