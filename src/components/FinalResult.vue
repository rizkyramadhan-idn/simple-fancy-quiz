<template>
  <div>
    <b-card>
      <b-card-body>
        <b-card-title>Final Result</b-card-title>
        <b-card-sub-title class="mb-2"
          >Score: {{ totalCorrectAnswers() }} / 10</b-card-sub-title
        >
        <b-card-text>
          <b-badge variant="success mr-2">Correct</b-badge>
          <b-badge variant="danger">Incorrect</b-badge>
        </b-card-text>
      </b-card-body>

      <b-list-group flush>
        <b-list-group-item
          v-for="(userAnswer, index) in userAnswers"
          :key="index"
          v-bind:class="[
            `${
              correctAnswers[index] === userAnswers[index]
                ? 'answer-correct'
                : 'answer-wrong'
            }`,
            'mb-2',
          ]"
          >Question {{ index + 1 }}</b-list-group-item
        >
      </b-list-group>
    </b-card>
  </div>
</template>

<script>
export default {
  props: ["userAnswers", "correctAnswers"],
  data() {},
  methods: {
    totalCorrectAnswers() {
      let total = 0;

      for (let i = 0; i < this.userAnswers.length; i++) {
        if (this.userAnswers[i] === this.correctAnswers[i]) {
          total += 1;
        }
      }

      return total;
    },
  },
  computed: {},
};
</script>

<style>
.answer-correct {
  background-color: #28a745;
  color: white;
  border: none;
}

.answer-wrong {
  background-color: #dc3545;
  color: white;
  border: none;
}
</style>
