<template>
  <GridComponent :currentGuess="currentGuess" :guesses="guesses" :turn="turn" />
  <KeypadComponent :usedKeys="usedKeys" />
  <ModalComponent
    v-if="showModal"
    :isCorrect="isCorrect"
    :turn="turn"
    :solution="solution"
  />
</template>

<script>
import GridComponent from "./GridComponent.vue";
import KeypadComponent from "./KeypadComponent.vue";
import ModalComponent from "./ModalComponent.vue";

export default {
  components: { GridComponent, KeypadComponent, ModalComponent },
  props: ["solution"],
  data() {
    return {
      turn: 0,
      currentGuess: "",
      guesses: [...Array(6)],
      history: [],
      isCorrect: false,
      usedKeys: {},
      showModal: false,
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

      let newKeys = { ...this.usedKeys };
      formattedGuess.forEach((l) => {
        const currentColor = newKeys[l.key];

        if (l.color === "green") {
          newKeys[l.key] = "green";
        }

        if (l.color === "yellow" && currentColor !== "green") {
          newKeys[l.key] = "yellow";
        }

        if (
          l.color === "grey" &&
          currentColor !== "green" &&
          currentColor !== "yellow"
        ) {
          newKeys[l.key] = "grey";
        }

        this.usedKeys = newKeys;
      });

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

    () => window.removeEventListener("keyup", this.handleKeyUp);
  },
  updated() {
    if (this.isCorrect) {
      setTimeout(() => (this.showModal = true), 2000);
      window.removeEventListener("keyup", this.handleKeyUp);
    }

    if (this.turn > 5) {
      setTimeout(() => (this.showModal = true), 2000);
      window.removeEventListener("keyup", this.handleKeyUp);
    }
  },
};
</script>

<style></style>
