<template>
    <div class="question-card">
        Question card
        <h3 v-if="currentQuestion">{{ currentQuestion.question }}</h3>
        <div class="answers-holder">
            <div class="answer" 
                :class="{'selected': selectedAnswerIndex === index}"
                v-for="(answer,index) in answers" :key="index"
                @click="selectAnswer(index)">
                {{ answer }}
            </div>
        </div>
        <button @click="next">Next</button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            selectedAnswerIndex: -1 
        }
    },
    props: {
        currentQuestion: Object,
        nextQuestion: Function,
    },
    methods: {
        next: function() {
            this.selectedAnswerIndex = -1
            this.nextQuestion()
        },
        selectAnswer: function(index) {
            console.log('selected answer', index)
            this.selectedAnswerIndex = index
            if (this.answers[index] === this.currentQuestion.correct_answer) {
                this.$emit('correct-answer')
            }
        }
    },
    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            // TODO Shuffle answers
            return answers
        }
    }
}
</script>

<style scoped>
.question-card {
    border: 3px solid rgba(0,0,0,0.3);
    border-radius: 3px;
    margin: 9px auto;
    padding: 12px;
    max-width: 550px;
}

.answers-holder {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    max-width: 450px;
    margin: 0 auto;
}

.answer {
    display: flex;
    flex-direction: row;
    border-bottom: 1px solid #4d4d4d;
    margin-bottom: 9px;
    padding: 3px 0px;
}

.answer:hover {
    cursor: pointer;
    background-color: rgba(0,0,0,0.05);
}

.answer.selected {
    border: 1px solid; 
    background-color: rgba(0,0,0,0.05);
}
</style>