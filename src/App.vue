<template>
  <div id="app">
    <div class="header">
      <h4>Categories</h4>
      <nav class="cat-nav">
        <ul class="categories-list">
          <li v-for="category in categories" :key="category.name">
            <a href="#" @click="selectCategory(category)">{{ category.name }}</a>
          </li>
        </ul>
      </nav>
    </div>
    <div v-if="loading">Loading...</div>
    <div class="no-q-alert" v-if="questions.length === 0">
      Select category to begin
    </div>
    <QuestionCnt
      v-if="questions.length !== 0"
      :totalQuestions="questions.length"
      :currentQuestion="currIndex + 1" />
    <QuestionCard
      v-if="questions.length !== 0 && hideQuestions === false"
      :currentQuestion="questions[currIndex]"
      :nextQuestion="nextQuestion"
      v-on:correct-answer="onCorrectAnswer"
      :submitAnswers="submitAnswers"/>

    <div class="resultbox" v-if="hideQuestions === true">
      You have {{ correctAnswers }} correct answers of {{ questions.length }} total!
    </div>
  </div>
</template>

<script>
import QuestionCard from './components/QuestionCard.vue'
import QuestionCnt from './components/QuestionCnt.vue'
export default {
  name: 'App',
  components: {
    QuestionCard,
    QuestionCnt,
  },
  data() {
    return {
      currentCategory: { name: '', id: -1 },
      categories: [
        { name: 'Any', id: '' },
        { name: 'Film', id: 11 },
        { name: 'History', id: 23 },
        { name: 'Animals', id: 27 },
      ],
      questions: [],
      loading: false,
      currIndex: 0,
      correctAnswers: 0,
      hideQuestions: false,
    }
  },
  methods: {
    selectCategory: function (category) {
      console.log(`Selected category ${category.name}(${category.id})`)
      this.currentCategory = category
      this.loading = true
      this.fetchQuestions()
    },
    fetchQuestions: function () {
      const URL = `https://opentdb.com/api.php?amount=10&category=${this.currentCategory.id}`
      fetch(URL, { method: 'GET' })
        .then(data => data.json())
        .then(response => {
          this.loading = false
          this.questions = response.results
          console.log(this.questions)
        })
        .catch(error => {
          this.loading = false
          console.log(error)
          // TODO Display error in a error box
        })
    },
    nextQuestion: function() {
      if (this.currIndex !== this.questions.length - 1) {
        this.currIndex++
      }
    },
    onCorrectAnswer: function() {
      this.correctAnswers++
      console.log('correct answer')
    },
    submitAnswers: function() {
      this.hideQuestions = true
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
}

.no-q-alert {
  background-color: #fffacd;
  max-width: 500px;
  margin: 9px auto;
  padding: 24px;
  border: 3px solid #f0e68c;
  border-radius: 6px;
  text-align: center;
}

.cat-nav {
  border-bottom: 1px solid #373737;
}

.categories-list li {
  display: inline-block;
  margin-right: 12px;
}

.categories-list li a {
  color: #3b57f8;
  border: 1px solid #3b57f8;
  padding: 3px 9px;
  text-decoration: none;
}

.categories-list li a:hover {
  background-color: rgba(0,0,0,0.05);
}
</style>
