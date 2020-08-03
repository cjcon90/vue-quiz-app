<template>
  <div class="app" id="app">
    <Header />
    <QuestionBox v-if="questions.length" :currentQuestion="questions[index]" :next="next" />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      index: 0,
    };
  },
  methods: {
    next() {
      this.index += 1;
    },
  },
  mounted: function () {
    fetch(
      "https://opentdb.com/api.php?amount=10&difficulty=medium&type=multiple",
      {
        method: "get",
      }
    )
      .then((res) => res.json())
      .then((data) => (this.questions = data.results));
  },
};
</script>

<style>
</style>

