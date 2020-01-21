<template>
  <div id="app">
    <ul class="stats">
      <li>No. of clicks: {{ clicks }}</li>
      <li>Matched pairs: {{ pairs }}</li>
      <li>First click: {{ first }}</li>
      <li>Second click: {{ second }}</li>
    </ul>

    <p>
      <button @click="resetGame">New game</button>
    </p>

    <ul class="cards" :class="[{ 'is-Waiting': !clickable }, { 'is-Inactive': win }]" :key="game">
      <Card
        v-for="n in 12"
        :key="n"
        :n="n"
        @click.native="checkCard(n)"
        :class="[{ 'is-Clicked': clicked.includes(n) }, { 'is-Revealed': pairs.includes(n) || pairs.includes(n-1) }]"
      />
    </ul>

    <transition name="fade">
      <div class="won" v-if="win">
        <h2>Congratulations!</h2>
        <p>Game finished in {{ clicks }} clicks.</p>
        <button @click="resetGame">New game</button>
      </div>
    </transition>
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
      second: null,
      clickable: true,
      clicked: [],
      game: 0
    };
  },
  methods: {
    checkCard(n) {
      this.clicks++; // increment clicks each time we reveal a card
      this.clicked.push(n); // check which cards are being clicked in current session

      if (n % 2 === 0) {
        // decrease index number for even cards to create pairs
        n = n - 1;
      }

      if (this.clicks % 2 === 0) {
        // check for every second click
        this.second = n; // define second clicked card in our current session

        if (this.first === this.second) {
          // if pairs match, add them to our array called pairs
          this.pairs.push(this.first);
        } else {
          // if pairs don't match reset stuff
          this.clickable = false; // disable clicks if two pairs are being shown that don't match

          setTimeout(() => {
            // set a timer to hide both cards that didn't match in our current session
            this.clickable = true;
            this.second = null;
            this.clicked = [];
          }, 1000);
        }
      } else {
        // this basically starts a new "click" session, meaning that we specify our first card and remove the second card from our previous session
        this.first = n;
        this.second = null;
      }
    },
    resetGame() {
      // self-explanatory. Resets all rules
      this.pairs = [];
      this.clicks = 0;
      this.first = this.second = null;
      this.clickable = true;
      this.clicked = [];
      this.game++; // increment the :key value to re-order all cards for a new game
    }
  },
  computed: {
    win() {
      // if all pairs have been revealed set winning conditions to true
      if (this.pairs.length === 6) return true;
      return false;
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
  grid-template-columns: repeat(3, minmax(200px, 1fr));
  grid-gap: 20px;
  padding: 0;
  max-width: 700px;
  margin: 0 auto;
  transition: 300ms;
}

.cards.is-Waiting {
  pointer-events: none;
}

.cards.is-Inactive {
  opacity: 0.5;
}

.stats {
  background: rgba(0, 0, 0, 0.9);
  padding: 20px;
  margin: 0;
  padding: 10px;
  color: white;
  font-size: 12px;
  position: fixed;
  top: 0;
  right: 0;
}

.stats li {
  display: block;
  text-align: left;
}

.won {
  position: fixed;
  padding: 30px;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: white;
  border-radius: 5px;
  z-index: 100;
  box-shadow: 0 0 30px rgba(0, 0, 0, 0.5);
  min-width: 300px;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 500ms;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  text-align: center;
  margin: 60px 0;
}
</style>
