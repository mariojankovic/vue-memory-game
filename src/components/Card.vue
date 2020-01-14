<template>
  <li class="card" :style="{ order: Math.floor(Math.random() * 100) + 1 }">
    <img :src="`https://picsum.photos/id/${cardID * 10}/200/200`" alt>
  </li>
</template>

<script>
export default {
  props: ["n"],
  computed: {
    cardID() {
      // return this.n % 2 === 0 ? this.n - 1 : this.n;

      if (this.n % 2 === 0) {
        return this.n - 1;
      } else {
        return this.n;
      }
    }
  }
};
</script>

