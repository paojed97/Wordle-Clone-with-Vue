<template>
  <div>current guess - {{ currentGuess }}</div>
</template>

<script>
export default {
  name: "WordleComponent",
  props: ["solution"],
  data() {
    return {
      turn: 0,
      currentGuess: "",
      setGuesses: [], // each guess is an array
      setHistory: [], // each guess is a string
      isCorrect: false,
    };
  },
  methods: {
    // format a guess into an array of letter objects
    // e.g. [{ key: 'a', color: 'yellow'}]
    formatGuess() {},

    // add a new guess to the guesses state
    // update the isCorrect state if the guess is correct
    // add one turn to the state
    addNewGuess() {},

    // handle keyup event & track current guess
    // if user presses enter, add the new guess
    handleKeyUp({ key }) {
      if (key === "Backspace") {
        this.currentGuess = this.currentGuess.slice(0, -1);
        return;
      }

      if (/^[a-zA-z]$/.test(key)) {
        if (this.currentGuess.length < 5) {
          this.currentGuess += key;
        }
      }
    },
  },
  mounted() {
    window.addEventListener("keyup", this.handleKeyUp);

    return () => window.removeEventListener("keyup", this.handleKeyUp);
  },
};
</script>

<style></style>
