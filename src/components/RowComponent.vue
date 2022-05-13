<template>
  <div class="row past" v-if="guess">
    <div :class="l.color" v-for="(l, i) in guess" :key="i">{{ l.key }}</div>
  </div>

  <div class="row current" v-else-if="currentGuess">
    <div class="filled" v-for="(letter, i) in currentGuess.split('')" :key="i">
      {{ letter }}
    </div>
    <div
      v-for="(_, i) in [...Array(5 - currentGuess.split('').length)]"
      :key="i"
    ></div>
  </div>

  <div class="row" v-else>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
  </div>
</template>

<script>
export default {
  props: ["guess", "currentGuess"],
};
</script>

<style>
.row {
  text-align: center;
  display: flex;
  justify-content: center;
}

.row > div {
  display: block;
  width: 60px;
  height: 60px;
  border: 1px solid #bbb;
  margin: 4px;
  text-align: center;
  line-height: 60px;
  text-transform: uppercase;
  font-weight: normal;
  font-size: 2em;
}

.row > div.green {
  --background: #528d4e;
  --border-color: #528d4e;
  animation: flip 0.5s ease forwards;
}

.row > div.grey {
  --background: #3a393c;
  --border-color: #3a393c;
  animation: flip 0.5s ease forwards;
}

.row > div.yellow {
  --background: #b49f39;
  --border-color: #b49f39;
  animation: flip 0.5s ease forwards;
}

.row > div:nth-child(2) {
  animation-delay: 0.2s;
}

.row > div:nth-child(3) {
  animation-delay: 0.4s;
}

.row > div:nth-child(4) {
  animation-delay: 0.6s;
}

.row > div:nth-child(5) {
  animation-delay: 0.8s;
}

.row.current > div.filled {
  animation: bounce 0.2s ease-in-out forwards;
}

/* animation */
@keyframes flip {
  0% {
    transform: rotateX(0);
    background: #121212;
    border-color: #3a393c;
  }
  45% {
    transform: rotateX(90deg);
    background: #121212;
    border-color: #3a393c;
  }
  55% {
    transform: rotateX(90deg);
    background: var(--background);
    border-color: var(--border-color);
    font-size: 2.5em;
  }
  100% {
    transform: rotateX(0);
    background: var(--background);
    border-color: var(--border-color);
    font-weight: bold;
    font-size: 2.5em;
  }
}

@keyframes bounce {
  0% {
    transform: scale(1);
    border-color: #ddd;
  }
  50% {
    transform: scale(1.2);
  }
  100% {
    transform: scale(1);
    border-color: white;
  }
}
</style>
