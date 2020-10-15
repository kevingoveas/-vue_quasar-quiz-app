<template>
  <div class="col">
    <q-card class="col">
      <q-card-section class="">
        <div class="text-h6">
          {{ activeQuestion.question }}
        </div>
        <q-list bordered separator>
          <q-item
            clickable
            v-ripple
            v-for="(answer, index) in answers"
            :key="index"
            :class="getHighlightClass(index)"
            class="borderClass"
          >
            <q-item-section @click="selectAnswer(index)">{{
              answer
            }}</q-item-section>
          </q-item>
        </q-list>
      </q-card-section>
      <q-separator />
      <q-card-actions align="right">
        <q-btn
          @click="submitAnswer"
          :disabled="selectedIndex === null || answered"
          >Submit</q-btn
        >
        <q-btn @click="next">Next</q-btn>
      </q-card-actions>
      <Footer :correct="correct" :total="total" />
    </q-card>
  </div>
</template>

<script>
import _ from "lodash";
import Footer from "./Footer";
export default {
  name: "QuestionBox",
  components: {
    Footer
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: [],
      correct: 0,
      total: 0,
      answered: false
    };
  },
  props: {
    activeQuestion: Object,
    next: Function
  },
  watch: {
    activeQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.shuffleAnswers();
      }
    }
  },
  computed: {
    answers() {
      let answers = [...this.activeQuestion.incorrect_answers];
      answers.push(this.activeQuestion.correct_answer);
      return answers;
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    submitAnswer() {
      let correctAnswer = false;
      if (this.selectedIndex === this.correctIndex) {
        this.correct++;
        correctAnswer = true;
      }
      this.total++;
      this.answered = true;
    },
    shuffleAnswers() {
      let answers = [
        ...this.activeQuestion.incorrect_answers,
        this.activeQuestion.correct_answer
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.activeQuestion.correct_answer
      );
    },
    getHighlightClass(index) {
      let highlightClass = "";
      if (!this.answered && this.selectedIndex === index) {
        highlightClass = "selected";
      } else if (this.answered && this.correctIndex === index) {
        highlightClass = "green";
      } else if (
        this.answered &&
        this.selectedIndex === index &&
        this.correctIndex !== index
      ) {
        highlightClass = "red";
      }
      return highlightClass;
    }
  }
};
</script>
<style lang="sass" scoped>
.col
  padding: 10px 30px 10px 30px
.borderClass
  border: 1px solid blue
.right
  margin:  0 10px 0 0
  margin-top: 10px;
.green
  background: green
.selected
  background: lightgreen
.gray
  background: lightgray
.red
  background: red
</style>
