<template>
  <div class="app-wrapper">
    
    <div class="app-body">
      <h1>KanaQuiz</h1>
      <div v-if="!selectedAlphabet" class="start-screen">
        <h2>Choose alphabet</h2>
        <div class="alphabet-btn-row">
          <button @click="selectAlphabet('katakana')" class="btn alphabet-btn">
            <div>カタカナ</div>
            <div>Katakana</div> 
          </button>
          <button @click="selectAlphabet('hiragana')" class="btn alphabet-btn">
            <div>ひらがな</div>
            <div>
              Hiragana
            </div>
          </button>
          <button @click="selectAlphabet('dakuten')" class="btn alphabet-btn">
            <div>だくてん</div>
            <div>
              Dakuten
            </div>
          </button>
        </div>
      </div>

    

    <div v-else class="quiz-bg shadow-lg">
      <div v-if="idx < questions.length">
        <div class="progressbar">
          <div
            v-for="(item, index) in questions"
            :key="index"
            class="p-item"
            :class="{
              'bg-green-300': item.guessedRight,
              'bg-red-300': item.guessedRight === false,
              'bg-gray-200': item.guessedRight === undefined,
            }"
          ></div>
        </div>

        <p class="title">{{ questions[idx].question }}</p>
        <div class="row">
          <div
            class="card"
            v-for="(answer, index) in shuffledAnswers"
            :key="index"
          >
            <label
              :for="`answer-${index}`"
              :class="[
                'option',
                { 'bg-green-200': selectedAnswer === answer && answer === questions[idx].correctAnswer }, 
                { 'bg-red-200': selectedAnswer === answer && answer !== questions[idx].correctAnswer }, 
                { 'bg-green-200': selectedAnswer && answer === questions[idx].correctAnswer && selectedAnswer !== answer }
              ]"
            >
              <input
                :id="`answer-${index}`"
                type="radio"
                class="hidden"
                :value="answer"
                v-model="selectedAnswer"
                @change="answered"
                :disabled="!!selectedAnswer"
              />
              {{ answer }}
            </label>
          </div>
        </div>

        <div class="mt-6" v-show="selectedAnswer">
          <button
            @click="nextQuestion"
            v-show="selectedAnswer && idx < questions.length - 1"
            class="btn next-btn"
          >
            Next
          </button>
          <button
            @click="showResults"
            v-show="selectedAnswer && idx === questions.length - 1"
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
            Correct Answers: <span class="text-2xl text-green-700 font-bold">{{ correctAnswers }}</span>
          </p>
          <p>
            Wrong Answers: <span class="text-2xl text-red-700 font-bold">{{ wrongAnswers }}</span>
          </p>
        </div>
        <div class="mt-6">
          <button @click="resetQuiz" class="btn play-again-btn">
            Play again
          </button>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: "KanaQuiz",
  data() {
  return {
    idx: 0,
    selectedAnswer: "",
    correctAnswers: 0,
    wrongAnswers: 0,
    questions: [],
    selectedAlphabet: "",
    fullAlphabet: [
      "a", "i", "u", "e", "o", 
      "ka", "ki", "ku", "ke", "ko",
      "sa", "shi", "su", "se", "so", 
      "ta", "chi", "tsu", "te", "to",
      "na", "ni", "nu", "ne", "no", 
      "ha", "hi", "fu", "he", "ho",
      "ma", "mi", "mu", "me", "mo", 
      "ya", "yu", "yo", 
      "ra", "ri", "ru", "re", "ro", 
      "wa", "wo", "n"
    ],
    dakutenAlphabet: [
      "ga", "gi", "gu", "ge", "go",
      "za", "ji", "zu", "ze", "zo",
      "da", "ji", "zu", "de", "do",
      "ba", "bi", "bu", "be", "bo",
      "pa", "pi", "pu", "pe", "po"
    ]
  };
},

  created() {
    // No questions loaded initially
  },
  computed: {
  shuffledAnswers() {
    if (!this.questions[this.idx]) return [];
    const currentQuestion = this.questions[this.idx];
    const answers = new Set([currentQuestion.correctAnswer]);

    // Use appropriate alphabet based on selectedAlphabet
    const alphabet = this.selectedAlphabet === 'dakuten' 
      ? this.dakutenAlphabet 
      : this.fullAlphabet;

    while (answers.size < 4) {
      const randomAnswer = alphabet[Math.floor(Math.random() * alphabet.length)];
      answers.add(randomAnswer);
    }

    return Array.from(answers).sort(() => Math.random() - 0.5);
  },
},

  methods: {
    async selectAlphabet(alphabet) {
      this.selectedAlphabet = alphabet;
      await this.loadQuestions();
      this.shuffleQuestions();
    },
    async loadQuestions() {
      const fileName = this.selectedAlphabet === 'katakana'
    ? 'katakana.json'
    : this.selectedAlphabet === 'hiragana'
    ? 'hiragana.json'
    : 'dakuten.json';
      try {
        const response = await import(`../${fileName}`);
        this.questions = response.default;
      } catch (error) {
        console.error("Failed to load questions:", error);
      }
    },
    shuffleQuestions() {
      this.questions = [...this.questions].sort(() => Math.random() - 0.5);
    },
    answered() {
      const currentQuestion = this.questions[this.idx];
      if (this.selectedAnswer === currentQuestion.correctAnswer) {
        this.correctAnswers++;
        currentQuestion.guessedRight = true;
      } else {
        this.wrongAnswers++;
        currentQuestion.guessedRight = false;
      }
      
      setTimeout(() => {
        this.nextQuestion();
      }, 2000);
    },
    nextQuestion() {
      if (this.idx < this.questions.length - 1) {
        this.idx++;
        this.selectedAnswer = "";
      }
    },
    showResults() {
      this.idx++;
    },
    resetQuiz() {
      this.idx = 0;
      this.selectedAnswer = "";
      this.correctAnswers = 0;
      this.wrongAnswers = 0;
      this.selectedAlphabet = "";
    },
  },
};
</script>
