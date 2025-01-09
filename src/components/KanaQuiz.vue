<template>
  <div>
    <h1>KanaQuiz</h1>
    <div class="quiz-bg shadow-lg">
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
                { 'bg-green-200': selectedAnswer === answer && answer === questions[idx].correctAnswer }, // подсветка правильного ответа
                { 'bg-red-200': selectedAnswer === answer && answer !== questions[idx].correctAnswer }, // подсветка выбранного неправильного ответа
                { 'bg-green-200': selectedAnswer && answer === questions[idx].correctAnswer && selectedAnswer !== answer } // подсветка правильного ответа, если выбран неправильный
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
</template>

<script>
import usersData from "../questions.json";

export default {
  name: "KanaQuiz",
  data() {
    return {
      idx: 0,
      selectedAnswer: "",
      correctAnswers: 0,
      wrongAnswers: 0,
      questions: [],
      fullAlphabet: [
        "a", "i", "u", "e", "o", "ka", "ki", "ku", "ke", "ko",
        "sa", "shi", "su", "se", "so", "ta", "chi", "tsu", "te", "to",
        "na", "ni", "nu", "ne", "no", "ha", "hi", "fu", "he", "ho",
        "ma", "mi", "mu", "me", "mo", "ya", "yu", "yo", "ra", "ri",
        "ru", "re", "ro", "wa", "wo", "n"
      ],
    };
  },
  created() {
    this.shuffleQuestions();
  },
  computed: {
    shuffledAnswers() {
      if (!this.questions[this.idx]) return [];
      const currentQuestion = this.questions[this.idx];
      const answers = new Set([currentQuestion.correctAnswer]);
      
      while (answers.size < 4) {
        const randomAnswer = this.fullAlphabet[Math.floor(Math.random() * this.fullAlphabet.length)];
        answers.add(randomAnswer);
      }
      
      return Array.from(answers).sort(() => Math.random() - 0.5);
    },
  },
  methods: {
    shuffleQuestions() {
      this.questions = [...usersData].sort(() => Math.random() - 0.5);
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
      
      // Добавляем задержку перед переходом к следующему вопросу
      setTimeout(() => {
        this.nextQuestion();
      }, 1500); // Переход через 2 секунды
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
      this.shuffleQuestions();
    },
  },
};
</script>
