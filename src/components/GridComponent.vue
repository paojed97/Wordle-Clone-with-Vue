<template>
  <div>
    <template v-for="(g, i) in guesses" :key="i">
      <RowComponent v-if="turn === i" :currentGuess="currentGuess" />
      <RowComponent v-else :guess="g" />
    </template>
  </div>
</template>

<script>
import RowComponent from "./RowComponent.vue";

export default {
  components: { RowComponent },
  props: ["currentGuess", "guesses", "turn"],
};
</script>

<style></style>
