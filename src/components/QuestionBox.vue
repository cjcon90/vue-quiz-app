<template>
  <div class="questionBox">
    <div class="questionBox__top">
      <div class="questionBox__score">
        <p class="questionBox__score--text">Score: {{ numCorrect}} / {{ numTotal }}</p>
      </div>
      <div class="questionBox__new">
        <button class="questionBox__new--button">
          <a href>New Quiz</a>
        </button>
      </div>
    </div>
    <div class="questionBox__quiz">
      <div class="questionBox__quiz--question">{{ question }}</div>
      <div class="questionBox__quiz--answers">
        <ul>
          <li
            v-for="(answer, index) in shuffledAnswers"
            :key="index"
            @click="selectedAnswer(index)"
            :class="[isCorrect === null && index === selectedIndex ? 'selected' :
            isCorrect !== null && answer === currentQuestion.correct_answer ? 'correct' :
            isCorrect === false && index === selectedIndex ? 'incorrect' : '']"
            :style="{ 'pointer-events': isCorrect !== null ? 'none' : null }"
          >{{ answer }}</li>
        </ul>
      </div>
    </div>
    <div class="questionBox__buttons">
      <button
        class="questionBox__buttons--submit"
        :disabled="selectedIndex === null || isCorrect !== null"
        @click="submitAnswer"
      >Submit</button>
      <button
        class="questionBox__buttons--next"
        :disabled="isCorrect === null || numTotal === 10"
        @click="next"
      >Next</button>
    </div>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    currentQuestion: Object,
    next: Function,
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: [],
      isCorrect: null,
      numCorrect: 0,
      numTotal: 0,
    };
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.isCorrect = null;
        this.shuffleAnswers();
      },
    },
  },
  methods: {
    selectedAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      this.shuffledAnswers = _.shuffle(this.answers);
    },
    submitAnswer() {
      if (this.selectedIndex === null) {
        return;
      } else if (this.shuffledAnswers[this.selectedIndex] === this.answers[3]) {
        this.isCorrect = true;
        this.numCorrect++;
        this.numTotal++;
      } else {
        this.isCorrect = false;
        this.numTotal++;
      }
    },
  },
  computed: {
    answers() {
      return [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
    },
    question() {
      return this.currentQuestion.question
        .replace(/&quot;/g, '"')
        .replace(/&#039;/g, "'")
        .replace(/&rsquo;/g, "'");
    },
  },
};
</script>