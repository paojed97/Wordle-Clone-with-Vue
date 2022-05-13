<template>
  <div class="row past" v-if="guess">
    <div :class="l.color" v-for="(l, i) in guess" :key="i">{{ l.key }}</div>
  </div>

  <div class="row current" v-else-if="currentGuess">
    <div class="filled" v-for="(letter, i) in currentGuess.split('')" :key="i">
      {{ letter }}
    </div>
    <div v-for="(_, i) in [...Array(5 - currentGuess.length)]" :key="i"></div>
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
  // data() {
  //   return {
  //     letters: null,
  //   };
  // },
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
  font-weight: bold;
  font-size: 2.5em;
}

.row > div.green {
  background: #528d4e;
  border-color: #528d4e;
}

.row > div.grey {
  background: #3a393c;
  border-color: #3a393c;
}

.row > div.yellow {
  background: #b49f39;
  border-color: #b49f39;
}
</style>
