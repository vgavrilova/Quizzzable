<template>
  <div>
    <b-nav>
      <b-nav-item disabled><b>Quizzzable</b></b-nav-item>
      <b-nav-item disabled>Counter: {{ numCorrect }}/{{ numTotal }}</b-nav-item>
    </b-nav>
    <hr class="my-4" />
  </div>
</template>

<script>
export default {
  props: {
    numCorrect: Number,
    numTotal: Number,
  },
};
</script>
