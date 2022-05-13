<template>
  <div>current guess - {{ currentGuess }}</div>
  <GridComponent :currentGuess="currentGuess" :guesses="guesses" :turn="turn" />
</template>

<script>
import GridComponent from "./GridComponent.vue";

export default {
  components: { GridComponent },
  props: ["solution"],
  data() {
    return {
      turn: 0,
      currentGuess: "",
      guesses: [...Array(6)],
      history: [],
      isCorrect: false,
    };
  },
  methods: {
    // format a guess into an array of letter objects
    // e.g. [{ key: 'a', color: 'yellow'}]
    formatGuess() {
      let solutionArray = [...this.solution];
      let formattedGuess = [...this.currentGuess].map((l) => {
        return { key: l, color: "grey" };
      });
      // find any green letters (letters in solution & correct position)
      formattedGuess.forEach((l, i) => {
        if (solutionArray[i] === l.key) {
          formattedGuess[i].color = "green";
          solutionArray[i] = null;
        }
      });
      // find any yellow letters (letters in solution but not in correct position)
      formattedGuess.forEach((l, i) => {
        if (solutionArray.includes(l.key) && l.color !== "green") {
          formattedGuess[i].color = "yellow";
          solutionArray[solutionArray.indexOf(l.key)] = null;
        }
      });
      return formattedGuess;
    },
    // add a new guess to the guesses state
    // update the isCorrect state if the guess is correct
    // add one turn to the state
    addNewGuess(formattedGuess) {
      if (this.currentGuess === this.solution) {
        this.isCorrect = true;
      }
      let newGuesses = [...this.guesses];
      newGuesses[this.turn] = formattedGuess;
      this.guesses = newGuesses;
      this.history = [...this.history, this.currentGuess];
      this.turn += 1;
      this.currentGuess = "";
    },
    // handle keyup event & track current guess
    // if user presses enter, add the new guess
    handleKeyUp({ key }) {
      if (key === "Enter") {
        // only add guess if turn is less than 5
        if (this.turn > 5) {
          console.log("You used all your guesses");
          return;
        }
        // do not allow duplicate words
        if (this.history.includes(this.currentGuess)) {
          console.log("You already tried that word");
          return;
        }
        // check word is 5 characters long
        if (this.currentGuess.length !== 5) {
          console.log("Word must be 5 characters long");
          return;
        }
        const formatted = this.formatGuess();
        this.addNewGuess(formatted);
      }
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
