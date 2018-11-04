<template>
  <div class="quiz dev">
    <div class="question">
      <p>{{question}}</p>
    </div>
    <div class="flex-container">
      <div class="answer" :class="getClass('a')" @click="!showResult ? answerHandler('a') : null">
        <input type="radio" value="a" v-model="answer" @change="answerHandler" :disabled="showResult">
        <label>{{answers.a}}</label>
      </div>
      <div class="answer" :class="getClass('b')" @click="!showResult ? answerHandler('b') : null">
        <input type="radio" value="b" v-model="answer" @change="answerHandler" :disabled="showResult">
        <label>{{answers.b}}</label>
      </div>
      <div class="answer" :class="getClass('c')"  @click="!showResult ? answerHandler('c') : null">
        <input type="radio" value="c" v-model="answer" @change="answerHandler" :disabled="showResult">
        <label>{{answers.c}}</label>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Question",
  props: {
    question: String,
    answers: Object,
    correctAnswers: String,
    index: Number,
    showResult: Boolean
  },
  data: function() {
    return {
      answer: null,
      incorrectAnswer: false
    };
  },
  methods: {
    answerHandler: function(e) {
      if (!e.target) {
        this.answer = e;
      }
      this.$emit("input", { value: this.answer, index: this.index });
    },
    getClass(item) {
      return {
        correct: this.showResult && this.correctAnswers === item,
        incorrect:
          this.showResult && this.incorrectAnswer && this.answer === item
      };
    }
  },
  watch: {
    showResult: function() {
      if (this.answer != this.correctAnswers) {
        this.incorrectAnswer = true;
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>

  @greenyellow: #adff2f;
  @red:         #ff0000;
  @white:       #FFFFFF; 

.correct {
  background-color: @greenyellow !important;
}

.incorrect {
  background-color: @red !important;
}

.quiz {
  width: 100%;
  height: 100%;
  grid-auto-flow: dense;
  align-items: center;
}

.question {
  text-align: center;
  background-color:  @white;
  padding: 3%;
  margin-bottom: 10px;
}

.answer {
  display: grid;
  grid-auto-flow: dense;
  align-items: center;
  padding: 2%;
  grid-template-columns: 10% 90%;
}

.answer > * {
  cursor: pointer;
}

.flex-container {
  display: flex;
  flex-direction: column;
}

.flex-container > div {
  background-color: @white;
  margin-bottom: 10px;
  line-height: 50px;
}
</style>
