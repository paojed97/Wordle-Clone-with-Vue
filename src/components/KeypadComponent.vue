<template>
  <div class="keypad" v-if="letters">
    <div :class="usedKeys[l.key]" v-for="l in letters" :key="l.key">
      {{ l.key }}
    </div>
  </div>
</template>

<script>
export default {
  props: ["usedKeys"],
  data() {
    return {
      letters: null,
    };
  },
  mounted() {
    fetch("http://localhost:3000/letters")
      .then((res) => res.json())
      .then((json) => {
        this.letters = json;
      });
  },
};
</script>

<style>
.keypad {
  max-width: 500px;
  margin: 20px auto;
}

.keypad > div {
  margin: 5px;
  width: 40px;
  height: 50px;
  background: grey;
  display: inline-block;
  border-radius: 6px;
  line-height: 50px;
}

.keypad > div.green {
  background: #528d4e;
  transition: all 0.3s ease-in;
}

.keypad > div.yellow {
  background: #b49f39;
  transition: all 0.3s ease-in;
}

.keypad > div.grey {
  background: #3a393c;
  transition: all 0.3s ease-in;
}
</style>
