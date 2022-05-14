<template>
  <div class="modal">
    <div v-if="isCorrect">
      <h1>You win!</h1>
      <p class="solution">{{ solution }}</p>
      <p>You found the solution in {{ turn }} guesses :)</p>
    </div>
    <div v-if="!isCorrect">
      <h1>Nevermind...</h1>
      <p class="solution">{{ solution }}</p>
      <p>Better luck next time! :)</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ["isCorrect", "turn", "solution"],
};
</script>

<style>
.modal {
  background: rgba(0, 0, 0, 0.7);
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
}

.modal div {
  max-width: 480px;
  background: #3a393c;
  padding: 40px;
  border-radius: 10px;
  margin: 10% auto;
  box-shadow: 2px 2px 10px rgba(255, 255, 255, 0.3);
}

.modal .solution {
  color: #ff004c;
  font-weight: bold;
  font-size: 0.8em;
  text-transform: uppercase;
  letter-spacing: 1px;
}
</style>
