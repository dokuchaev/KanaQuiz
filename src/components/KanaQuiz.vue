<template>

  
  <div >
        <h1 >
          KanaQuiz
        </h1>
        <div class="quiz-bg shadow-lg ">

          

          


          
          <div v-if="idx < count">

            

      <div v-for="questionw in randomQuestions" :key="questionw.id">
      </div>


   
      <div class="progressbar" >
            <div v-for="item in questions" :key="item.id"
            
            class="p-item" 
            :class="{
             'bg-green-300': item.guessedRight,
              'bg-red-300': item.guessedRight == false,
              'bg-gray-200': !item.guessedRight,
            
            }"
            
       
            >
          
            </div>
         
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
           
            <div class="mt-6">
              <button
                @click="nextQuestion"
                v-show="selectedAnswer != '' && idx < count - 1"
                class="btn next-btn"
              >
                Next
              </button>
              <button
                @click="showResults"
                v-show="selectedAnswer != '' && idx == count - 1"
                class="btn"
              >
                Finish
              </button>
            </div>
          </div>
          <div v-else>
            <h2 class="result-title">Results</h2>
            <div class="result-text">
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
            <div class="mt-6">
              <button
                @click="resetQuiz"
                class="btn play-again-btn"
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
    msg: String,
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
        this.questions[this.idx].guessedRight = true;
      } else {
        this.wrongAnswers++;
        this.questions[this.idx].guessedRight = false;
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
      this.questions.sort(() => Math.random() - 0.5);
      
      this.questions[this.idx].guessedRight = null;
      this.questions.forEach((el) => ( 
        el.guessedRight = null,
        console.log(el)
        
        ));

     
  
    },

 
    
   
  },
 
}

</script>
