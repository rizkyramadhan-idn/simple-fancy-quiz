<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled>
        <b>Fancy Quiz App</b>
      </b-nav-item>

      <b-nav-item v-if="isAnswerSubmitted" disabled
        >Quiz is finished!</b-nav-item
      >
      <b-nav-item v-else disabled>Questions: {{ index + 1 }}/10</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: ["index", "isAnswerSubmitted", "totalCorrectAnswers"],
};
</script>
