<template>
  <div id="app">
    <b-container class="mt-5 mb-5" v-if="questions.length > 0">
      <Header
        v-bind:index="index"
        v-bind:isAnswerSubmitted="isAnswerSubmitted"
      />

      <b-container class="mt-5">
        <b-row>
          <b-col class="col-sm-12 col-md-10 col-lg-8 mx-auto">
            <FinalResult
              v-if="isAnswerSubmitted"
              v-bind:userAnswers="userAnswers"
              v-bind:correctAnswers="correctAnswers"
              v-bind:totalCorrectAnswers="totalCorrectAnswers"
            />

            <QuestionBox
              v-else
              v-bind:index="index"
              v-bind:currentQuestion="questions[index]"
              v-bind:addUserAnswer="addUserAnswer"
              v-bind:addCorrectAnswer="addCorrectAnswer"
              v-bind:isQuestionAnswered="isQuestionAnswered"
              v-bind:toggleAnswerQuestion="toggleAnswerQuestion"
              v-bind:submitAnswer="submitAnswer"
              v-bind:next="next"
            />
          </b-col>
        </b-row>
      </b-container>
    </b-container>

    <b-container class="mt-5 mb-5" v-else>
      <div class="text-center">
        <b-spinner label="Loading..."></b-spinner>
        <p>Loading ...</p>
      </div>
    </b-container>
  </div>
</template>

<script>
import _ from "lodash";
import Swal from "sweetalert2";

import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";
import FinalResult from "./components/FinalResult.vue";

export default {
  name: "App",
  data() {
    return {
      questions: [],
      userAnswers: [],
      correctAnswers: [],
      isQuestionAnswered: false,
      isAnswerSubmitted: false,
      totalCorrectAnswers: 0,
      index: 0,
    };
  },
  components: {
    Header,
    QuestionBox,
    FinalResult,
  },
  methods: {
    toggleAnswerQuestion(value) {
      this.isQuestionAnswered = value;
    },
    next() {
      if (this.index + 1 != this.userAnswers.length) {
        Swal.fire(
          "Warning",
          "Jawaban harus dipilih untuk melanjutkan!",
          "warning"
        );
      } else {
        this.index += 1;
        this.toggleAnswerQuestion(false);
      }
    },
    addUserAnswer(value) {
      this.userAnswers.push(value);
    },
    addCorrectAnswer(value) {
      this.correctAnswers.push(value);
    },
    submitAnswer() {
      this.isAnswerSubmitted = !this.isAnswerSubmitted;
    },
  },
  mounted() {
    fetch(
      "https://opentdb.com/api.php?amount=10&category=27&difficulty=easy&type=multiple",
      {
        method: "get",
      }
    )
      .then((response) => response.json())
      .then((result) => {
        this.questions = _.shuffle(result.results);
      });
  },
};
</script>
