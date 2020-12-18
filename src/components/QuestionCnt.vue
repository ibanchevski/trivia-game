<template>
    <div class="question-number-holder">
        {{ currentQuestion }}
        <span class="separator">&bsol;</span>
        {{ totalQuestions }}
    </div>
</template>

<script>
export default {
    props: {
        currentQuestion: Number,
        totalQuestions: Number,
    }
}
</script>

<style scoped>
.question-number-holder {
    max-width: 50px;
    margin: 9px auto;
    text-align: center;
    background: rgba(0,0,0,.1);
    padding: 9px 3px;
    border-radius: 3px;
}
.question-number-holder .separator {
    font-weight: bold;
}
</style>