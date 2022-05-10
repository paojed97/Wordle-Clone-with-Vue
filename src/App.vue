<template>
  <div class="app">
    <h1>Wordle</h1>
    <div v-if="solution">Solution is {{ solution }}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      solution: null,
    };
  },
  mounted() {
    fetch("http://localhost:3000/solutions")
      .then((res) => res.json())
      .then((json) => {
        // random int between 0 & 14
        const randomSolution = json[Math.floor(Math.random() * json.length)];
        this.solution = randomSolution.word;
      });
  },
};
</script>

<style>
body {
  text-align: center;
  font-size: 1em;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  margin: 0;
}

h1 {
  font-size: 1.2em;
  padding: 20px 0;
  border-bottom: 1px solid #eee;
  margin: 0 0 30px 0;
  color: #333;
}
</style>
