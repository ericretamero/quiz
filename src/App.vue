<template>
<div>
    <div class="gridcontainer">
        <div class="griditem1">
            <h2 id="responsive_headline">THE&nbsp;MALTA QUIZ</h2>
            <hr>
            <h3 class="subtitle">A simple quiz with 5 question about Malta </h3>
        </div>

        <div class="griditem2">
             <Parallax></Parallax>
        </div>

        <div class="griditem3">
    
          <Question v-for="(item, index) in questions" 
            :key=index  
            :question=item.question 
            :answers=item.answers
            :correctAnswers=item.correctAnswers
            :index=index 
            :showResult=showAnswerResult
            @input="onSaveAnswer">
          </Question>  

          <div style="margin-bottom: 10px;">
            <button :disabled="disabledButton" @click="onCheckResult">Send</button>
          </div>
              
        </div>

        <div class="griditem4">
            <Parallax></Parallax>
        </div>

    </div>
    <Modal v-if="showModalResult" @close="onShowModal" :msg="modalText"></Modal>
</div>


</template>

<script>
import Question from "./components/Question.vue";
import Parallax from "./components/Parallax.vue";
import Modal from "./components/Modal.vue";
import json from "./assets/data/quiz.json";

export default {
  name: "app",
  components: {
    Question,
    Parallax,
    Modal
  },
  props: {
    answers: Array
  },
  data: function() {
    return {
      questions: json,
      solutionAnswer: [],
      maxQuestion: 5,
      showModalResult: false,
      userAnswer: [],
      disabledButton: true,
      showAnswerResult: false,
      ranking: [],
      modalText: {
        correct: 0,
        total: 0
      }
    };
  },
  mounted() {
    console.log("App mounted!");

    if (localStorage.getItem("ranking")) {
      this.ranking = JSON.parse(localStorage.getItem("ranking"));
    }

    this.questions.forEach(question =>{
      this.solutionAnswer.push(question.correctAnswers);
    });
    
  },
  methods: {
    onCheckResult: function() {
      let correct = 0;
      this.userAnswer.forEach((answer, index) => {
        if (this.solutionAnswer[index] === answer) {
          correct++;
        }
      });

      const result = this.ranking.filter(num => num < correct);
      this.ranking.push(correct);
      this.ranking.sort();
      const total = ((result.length / this.ranking.length) * 100).toFixed(2);

      this.modalText = {
        correct,
        total
      };

      localStorage.setItem("ranking", JSON.stringify(this.ranking));

      this.showAnswerResult = true;
      this.showModalResult = true;
    },

    onSaveAnswer: function(values) {
      this.userAnswer[values.index] = values.value;
      this.disabledButton =
        this.userAnswer.length !== this.maxQuestion ||
        this.userAnswer.includes(undefined);
    },
    onShowModal: function(show) {
      this.showModalResult = !show;
      this.disabledButton = true;
    }
  },
  computed: {
    isAllSelected: function() {
      return (
        this.userAnswer.length !== this.maxQuestion ||
        this.userAnswer.includes(undefined)
      );
    }
  }
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
  font-family: "lato", helvetica, arial;
  line-height: 1.5;
  background-color: #faf8f6;
  font-family: "EB Garamond", serif;
}

.gridcontainer {
  display: grid;
  margin: auto;
  max-width: 990px;
  margin: 0 auto;
  padding-left: 4%;
  padding-right: 4%;
  justify-content: center;
  grid-template-rows: auto;
  grid-template-columns: 25% 50% 25%;
}

.griditem1 {
  grid-column: 2 / 3;
  margin: 0 auto;
  padding: 0;
  width: 100%;
  text-align: right;
}

.griditem2 {
  grid-column: 1 / 4;
  margin: 0 auto;
  padding: 0;
  width: 100%;
}

.griditem3 {
  grid-column: 2 / 3;
}

.griditem4 {
  grid-column: 1 / 4;
  margin: 0 auto;
  padding: 0;
  width: 100%;
}

h2 {
  margin: 0 auto;
  padding: 0;
  font-family: helvetica;
}

hr {
  margin: 0;
  padding: 0;
}

.subtitle {
  font-size: 1.3rem;
  margin-top: 20px;
  margin-bottom: 20px;
  font-style: italic;
  font-weight: 400;
  text-align: center;
}

button {
  padding: 10px;
  width: 100%;
}

article {
  margin-bottom: 10px;
}

/* Responsive */
@media (max-width: 500px) {
  .griditem3 {
    grid-column: 1 / 4;
  }
}
</style>
