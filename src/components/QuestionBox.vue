<template>
  <div>
    <b-jumbotron>
      <template v-slot:lead>
        <h4 v-html="`${index + 1} - ${currentQuestion.question}`"></h4>
      </template>

      <hr class="my-4" />

      <div class="mb-4">
        <b-list-group>
          <b-list-group-item
            v-for="(answer, index) in currentAnswers"
            :key="index"
            v-on:click="selectAnswer(index)"
            v-bind:class="[
              'answer',
              `${
                selectedAnswer === index && isQuestionAnswered
                  ? 'answer-selected'
                  : ''
              }`,
            ]"
          >
            <span v-html="`${getSpecificAlphabet(index)}. ${answer}`"></span>
          </b-list-group-item>
        </b-list-group>
      </div>

      <div class="float-right">
        <b-button
          v-if="index === 9"
          v-on:click="submitAnswer"
          variant="primary"
          href="#"
          class="mr-2"
          >Submit</b-button
        >
        <b-button v-else v-on:click="next" variant="success" href="#"
          >Next</b-button
        >
      </div>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: [
    "index",
    "currentQuestion",
    "addUserAnswer",
    "addCorrectAnswer",
    "isQuestionAnswered",
    "toggleAnswerQuestion",
    "submitAnswer",
    "next",
  ],
  data() {
    return {
      selectedAnswer: null,
      correctAnswer: null,
    };
  },
  methods: {
    selectAnswer(value) {
      this.selectedAnswer = value;

      this.addUserAnswer(this.selectedAnswer);
      this.addCorrectAnswer(
        this.currentAnswers.indexOf(this.currentQuestion.correct_answer)
      );

      this.toggleAnswerQuestion(true);
    },
    getSpecificAlphabet(index) {
      return ["A", "B", "C", "D"][index];
    },
  },
  computed: {
    currentAnswers() {
      return _.shuffle([
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ]);
    },
  },
};
</script>

<style>
.answer:hover {
  background-color: #007bff;
  color: white;
  cursor: pointer;
  border: none;
}

.answer-selected {
  background-color: #007bff;
  color: white;
  border: none;
}
</style>
