<script setup>
import { ref, computed } from 'vue'

const questions = ref([
    {
        question: 'Which of the following statement best define Vue.js?',
        answer: 2,
        options: [
            'Vue.js is an open-source JavaScript library that is used for developing user interfaces.',
            'Vue.js is an open-source front-end JavaScript framework used for developing user interfaces.',
            'Vue.js is an open-source, cross-platform, JavaScript run-time situation that performs the JavaScript program outside a web browser.'
        ],
        selected: null
    },
    {
        question: 'Why is Vue.js called a progressive framework?',
        answer: 1,
        options: [
            'Vue.js is called a progressive framework because it is being changed and developed continually.',
            'Vue.js is called a progressive framework because it facilitates us to create Dynamic User Interfaces and single-page applications.',
            'Vue.js is called a progressive framework because it follows the latest JavaScript standards.'
        ],
        selected: null
    },
    {
        question: 'What is the main usage of Vue.js?',
        answer: 1,
        options: [
            'Vue.js is a dynamic JavaScript framework that is frequently used for developing user interfaces.',
            'Vue.js is a JavaScript library that makes user interfaces for single-page applications by dividing UI into components.',
            'Vue.js uses the MVVM pattern to bind data to certain DOM elements.'
        ],
        selected: null
    },
    {
        question: 'Which of the following data binding interpolation is also known as "Mustache" syntax?',
        answer: 3,
        options: [
            'v-on',
            'v-model',
            '{{}}'
        ],
        selected: null
    },
    {
        question: 'Which of the following is the correct syntax to use for loop in Vue.js?',
        answer: 2,
        options: [
            'vFor',
            'v-for',
            '*v-for'
        ],
        selected: null
    }
])

const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() => {
    let value = 0
    questions.value.map(q => {
        if (q.selected != null && q.answer == q.selected) {
            value++
        }
    })
    return value
})

const getCurrentQuestion = computed(() => {
    let question = questions.value[currentQuestion.value]
    question.index = currentQuestion.value
    return question
})

function setCookie(cname, cvalue, exdays) {
    const d = new Date();
    d.setTime(d.getTime() + (exdays * 24 * 60 * 60 * 1000));
    let expires = "expires=" + d.toUTCString();
    document.cookie = cname + "=" + cvalue + ";" + expires + ";path=/";
}

function getCookie(cname) {
    let name = cname + "=";
    let decodedCookie = decodeURIComponent(document.cookie);
    let ca = decodedCookie.split(';');
    for (let i = 0; i < ca.length; i++) {
        let c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

const SetAnswer = (e) => {
    questions.value[currentQuestion.value].selected = e.target.value
    e.target.value = null
}

const NextQuestion = () => {
    if (currentQuestion.value < questions.value.length - 1) {
        currentQuestion.value++
        return
    }

    quizCompleted.value = true
}

const currentScore = computed(() => {
    let existingScores = getCookie('scores')
    let scores = existingScores.split(',').map(Number)
    scores.push(score.value)
    scores.sort((a, b) => a - b)
    setCookie('scores', scores, 999)

    let indexResult = scores.indexOf(score.value)

    return indexResult
})

</script>

<template>
    <main class="app">
        <h1>The Quiz</h1>

        <section class="quiz" v-if="!quizCompleted">
            <div class="quiz-info">
                <span class="question">{{ getCurrentQuestion.question }}</span>
            </div>

            <div class="options">
                <label v-for="(option, index) in getCurrentQuestion.options" :for="'option' + index" :class="`option ${getCurrentQuestion.selected == index
                    ? 'selected'
                    : ''
                }`">
                    <input type="radio" :id="'option' + index" :name="getCurrentQuestion.index" :value="index"
                        v-model="getCurrentQuestion.selected" @change="SetAnswer" />
                    <span>{{ option }}</span>
                </label>
            </div>

            <button @click="NextQuestion" :disabled="!getCurrentQuestion.selected">
                {{
                    getCurrentQuestion.index == questions.length - 1
                        ? 'Finish'
                        : getCurrentQuestion.selected == null
                            ? 'Select an option'
                            : 'Next question'
                }}
            </button>
        </section>

        <section v-else>
            <h2>You have finished the quiz!</h2>
            <p>Your score is {{ score }}/{{ questions.length }}</p>
            <p>You did better than {{ currentScore }} users</p>
        </section>
    </main>
</template>