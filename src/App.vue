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
    <QuestionCard
      v-if="questions.length !== 0"
      :currentQuestion="questions[currIndex]"
      :nextQuestion="nextQuestion"
      v-on:correct-answer="onCorrectAnswer"/>
  </div>
</template>

<script>
import QuestionCard from './components/QuestionCard.vue'

export default {
  name: 'App',
  components: {
    QuestionCard
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
