<template>
  <div>
    <q-card class="col">
      <Header />
      <QuestionBox
        v-if="questions.length"
        :activeQuestion="questions[index]"
        :next="next"
      />
    </q-card>
  </div>
</template>

<script>
import Vue from "vue";
import Header from "../components/Header";
import QuestionBox from "../components/QuestionBox";
export default {
  name: "MainLayout",
  components: { Header, QuestionBox },
  data() {
    return {
      questions: [],
      index: 0
    };
  },
  mounted() {
    //fetch("http://httpstat.us/500") //This is a super simple service for generating different HTTP codes.
    fetch("https://opentdb.com/api.php?amount=10&category=23&type=multiple", {
      method: "get"
    })
      .then(response => {
        if (!response.ok) {
          throw new Error(response.statusText);
        }
        return response.json();
      })
      .then(jsonData => {
        this.questions = jsonData.results;
      })
      .catch(e => {
        console.log(e);
      });
  },
  methods: {
    next() {
      this.index++;
    },
    getVueVersion() {
      //var version = Number(Vue.version.split('.')[0]);
      return Vue.version;
    }
  }
};
</script>

<style lang="sass" scoped>
.nav-col
  border: 1px solid rgba(0,0,0,0.12)
  box-shadow: 0 2px 4px 0 rgba(0,0,0,0.12)
  z-index: 1
.col
  padding: 10px 10px 10px 10px
  border: 1px solid rgba(0,0,0,0.12)
</style>
