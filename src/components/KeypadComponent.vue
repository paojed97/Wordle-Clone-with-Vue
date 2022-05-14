<template>
  <div class="keypad" v-if="topLetters">
    <div class="keys">
      <div :class="usedKeys[t.key]" v-for="t in topLetters" :key="t.key">
        {{ t.key }}
      </div>
    </div>
    <div class="keys">
      <div :class="usedKeys[m.key]" v-for="m in middleLetters" :key="m.key">
        {{ m.key }}
      </div>
    </div>
    <div class="keys">
      <div :class="usedKeys[b.key]" v-for="b in bottomLetters" :key="b.key">
        {{ b.key }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["usedKeys"],
  data() {
    return {
      topLetters: null,
      middleLetters: null,
      bottomLetters: null,
    };
  },
  mounted() {
    fetch("http://localhost:3000/topKeys")
      .then((res) => res.json())
      .then((json) => {
        this.topLetters = json;
      });

    fetch("http://localhost:3000/middleKeys")
      .then((res) => res.json())
      .then((json) => {
        this.middleLetters = json;
      });

    fetch("http://localhost:3000/bottomKeys")
      .then((res) => res.json())
      .then((json) => {
        this.bottomLetters = json;
      });
  },
};
</script>

<style>
.keypad {
  max-width: 700px;
  max-height: 300px;
  margin: 20px auto;
  text-transform: uppercase;
  font-size: 1em;
}

.keys {
  flex: 33%;
  display: flex;
  flex-direction: row;
  justify-content: center;
  margin: 5px;
}

.keys > div {
  width: 30px;
  height: 50px;
  margin: 5px;
  background: grey;
  border-radius: 4px;
  display: grid;
  place-items: center;
  cursor: pointer;
}

.keypad > .keys > div.green {
  background: #528d4e;
  transition: all 0.3s ease-in;
}

.keypad > .keys > div.yellow {
  background: #b49f39;
  transition: all 0.3s ease-in;
}

.keypad > .keys > div.grey {
  background: #3a393c;
  transition: all 0.3s ease-in;
}
</style>
