<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template #lead>
        {{ decodeQuestion }}
      </template>

      <hr class="my-4" />

      <b-list-group v-for="(answer, index) in answersArr" :key="answer + index">
        <b-list-group-item
          button
          @click="select(index)"
          :class="{
            selected: !answered && selectedIndex === index,
            correct: answered && index === correctIndex,
            incorrect:
              answered && index === selectedIndex && index !== correctIndex,
          }"
          >{{ answer }}</b-list-group-item
        >
      </b-list-group>

      <b-button
        variant="primary"
        @click="submitQuestion"
        :disabled="selectedIndex === null || answered"
        >Submit</b-button
      >
      <b-button
        variant="success"
        href="#"
        @click="btnNext"
        :disabled="selectedIndex === null || !answered"
        >Next ></b-button
      >
    </b-jumbotron>
  </div>
</template>

<script charset="utf-8">
export default {
  props: {
    currentQuestion: Object,
    btnNext: Function,
    answers: Array,
    increment: Function,
  },
  data() {
    return {
      selectedIndex: null,
      answered: false,
      correctIndex: 0,
    };
  },
  methods: {
    select(index) {
      this.correctIndex = this.answersArr.indexOf(
        atob(this.currentQuestion.correct_answer)
      );
      this.selectedIndex = index;
    },
    shuffleArray(array) {
      for (var i = array.length - 1; i > 0; i--) {
        var j = Math.floor(Math.random() * (i + 1));
        var temp = array[i];
        array[i] = array[j];
        array[j] = temp;
      }
      return array;
    },
    submitQuestion() {
      // if selected answer is a correct answer
      // return true
      let isCorrect;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.increment(isCorrect);
      this.answered = true;
      return isCorrect;
    },
  },
  watch: {
    currentQuestion() {
      this.selectedIndex = null;
      this.answered = false;
    },
  },
  computed: {
    answersArr() {
      const answers = [
        this.currentQuestion.correct_answer,
        ...this.currentQuestion.incorrect_answers,
      ].map((answer) => {
        return atob(answer);
      });
      return this.shuffleArray(answers);
    },
    decodeQuestion() {
      return atob(this.currentQuestion.question);
    },
  },
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.btn {
  margin: 0 5px;
}
.selected {
  background-color: rgb(153, 181, 214);
  color: white;
}
.correct {
  background-color: rgb(170, 214, 153);
}
.incorrect {
  background-color: rgb(214, 157, 153);
}
</style>
