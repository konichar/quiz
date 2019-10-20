<template>
   <div class="question-box-container">
       <b-jumbotron>

           <template slot="lead">
               {{ currentQuestion.question }}
           </template>

           <hr class="my-4" />

            <b-list-group>
                <b-list-group-item 
                v-for="(answer, index) in shuffleAnswers" 
                :key="index"
                @click="selectAnswer(index)"
                :class="[selectedIndex == index ? 'selected' : '' ]"
                >
                    {{answer}}
                </b-list-group-item>
            </b-list-group>
           <b-button 
           variant="primary"
           @click="submitAnswer"
           >Submit</b-button>
           <b-button @click="next" variant="success" href="#">Next</b-button>
       </b-jumbotron>
   </div>
</template>

<script>
import _ from 'lodash'
export default {
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function
    },
    data() {
        return {
            selectedIndex: null,
            shuffleAnswers: []
        }
    },
    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }
    },
    watch: {
        currentQuestion: {
            immediate: true,
            handler(){
                this.selectIndex = null,
                this.shuffleAnswer()
            }
        }
    },
    methods: {
        selectAnswer(index) {
            this.selectedIndex = index
        },
        shuffleAnswer(){
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffleAnswers = _.shuffle(answers)
        },
        submitAnswer(){
            let isCorrect = false
            if (this.selectedIndex == this.correctIndex) {
                isCorrect = true
            }
            this.increment(isCorrect)
        }
        
    },
}
</script>

<style scoped>
.list-group {
    margin-bottom: 15px;
}
.list-group-item:hover {
    background: #EEE;
    cursor: pointer;

}
.btn {
    margin: 0 5px;
}

.selected {
    background-color: blue;
}
.correct {
    background-color: green;
}
.incorrect {
    background-color: red;
}
</style>