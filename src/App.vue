<template>
  <div id="app">
    <div class="header">
      <h4>Categories</h4>
      <nav>
        <ul>
          <li v-for="category in categories" :key="category.name">
            <a href="#" @click="selectCategory(category)">{{ category.name }}</a>
          </li>
        </ul>
      </nav>
    </div>
  </div>
</template>

<script>
// import Header from './components/Header.vue'

export default {
  name: 'App',
  components: {

  },
  data() {
    return {
      URL: 'https://opentdb.com/api.php?amount=10&category=',
      categories: [
        { name: 'Any', id: '' },
        { name: 'Film', id: 11 },
        { name: 'History', id: 23 },
        { name: 'Animals', id: 27 },
      ],
      questions: []
    }
  },
  methods: {
    selectCategory: function (category) {
      console.log(`Selected category ${category.name}(${category.id})`)
      this.URL += category.id
      this.fetchQuestions()
    },
    fetchQuestions: function() {
      fetch(this.URL, { method: 'GET' })
        .then(data => data.json())
        .then(response => {
          this.questions = response.results;
        })
        .catch(error => {
          console.log(error)
          // TODO Display error in a error box
        })
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
