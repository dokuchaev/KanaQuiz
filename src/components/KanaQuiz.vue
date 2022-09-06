<template>
  <div >
        <h1 >
          KanaQuiz
        </h1>
        <div class="quiz-bg shadow-lg ">
          
          <div v-if="idx < count">
            

      <div v-for="questionw in randomQuestions" :key="questionw.id">
      </div>
            <p class="title">{{questions[idx]['question']}}</p>
            <div class="row">
             

              <div class="card"  v-for="[ index, answer ] in shuffledAnswers" :key="answer.id">
            <label
  
              :key="index"
              :for="index"
              class=""
              :class="[{'hover:bg-gray-100 cursor-pointer' : selectedAnswer == ''}, {'bg-green-200' : index == questions[idx].correctAnswer && selectedAnswer != ''}, {'bg-red-200' : selectedAnswer == index}]"
            >
              <input
                :id="index"
                type="radio"
                class="hidden"
                :value="index"
                @change="answered($event)"
                :disabled="selectedAnswer != ''"
              />
              {{answer}}
              
            </label>
          </div>
          </div>
           
            <div class="mt-6 flow-root">
              <button
                @click="nextQuestion"
                v-show="selectedAnswer != '' && idx < count - 1"
                class="float-right bg-indigo-600 text-white text-sm font-bold tracking-wide rounded-full px-5 py-2"
              >
                Next &gt;
              </button>
              <button
                @click="showResults"
                v-show="selectedAnswer != '' && idx == count - 1"
                class="float-right bg-indigo-600 text-white text-sm font-bold tracking-wide rounded-full px-5 py-2"
              >
                Finish
              </button>
            </div>
          </div>
          <div v-else>
            <h2 class="text-bold text-3xl">Results</h2>
            <div class="flex justify-start space-x-4 mt-6">
              <p>
                Correct Answers:
                <span class="text-2xl text-green-700 font-bold"
                  >{{correctAnswers}}</span
                >
              </p>
              <p>
                Wrong Answers:
                <span class="text-2xl text-red-700 font-bold"
                  >{{wrongAnswers}}</span
                >
              </p>
            </div>
            <div class="mt-6 flow-root">
              <button
                @click="resetQuiz"
                class="float-right bg-indigo-600 text-white text-sm font-bold tracking-wide rounded-full px-5 py-2"
              >
                Play again
              </button>
            </div>
          </div>
        </div>
      </div>
</template>

<script>




// function shuffleArray(array) {
//   for (let i = array.length - 1; i > 0; i--) {
//     const j = Math.floor(Math.random() * (i + 1))
//     const temp = array[i]
//     array[i] = array[j]
//     array[j] = temp
//   }
// }

import usersData from "../questions.json";
export default {
  name: 'KanaQuiz',
  props: {
    msg: String
  },

 
  data() {
    return {
      idx: 0,
      selectedAnswer: "",
      correctAnswers: 0,
      wrongAnswers: 0,
      count: 5,
      questions: usersData,
    };
    
  },
  computed:{
    randomQuestions () {
    usersData.sort(() => Math.random() - 0.5)
    return false
  },

  shuffledAnswers() {
    const shuffledAnswers = [];

    for (
      const answers = Object.entries(this.questions[this.idx].answers);
      answers.length;
      shuffledAnswers.push(answers.splice(Math.random() * answers.length | 0, 1)[0])
    ) ;

    return shuffledAnswers;
  },

},

  
  methods: {
    answered(e) {
      this.selectedAnswer = e.target.value;
      if (this.selectedAnswer == this.questions[this.idx].correctAnswer) {
        this.correctAnswers++;
      } else {
        this.wrongAnswers++;
      }
      
    },
    nextQuestion() {
      this.idx++;
      this.selectedAnswer = "";
      document.querySelectorAll("input").forEach((el) => (el.checked = false));
      
    },
    showResults() {
      this.idx++;
      
    },
    resetQuiz() {
      this.idx = 0;
      this.selectedAnswer = "";
      this.correctAnswers = 0;
      this.wrongAnswers = 0;
      this.questions.sort(() => Math.random() - 0.5)
      
    },
  },

}

</script>
