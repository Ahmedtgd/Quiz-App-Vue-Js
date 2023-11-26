<script setup>
import{ref, computed }from 'vue'

const questions = ref([
{
question:'what is this ?',
answer:1,
options:[
'A',
'B',
'c'
],
selected: null
},
{
question:'what is that ?',
answer:2,
options:[
'q',
'w',
'z'
],
selected: null
}
])

const quizCompleted = ref(false)
const currentQuestion= ref(0)

const score = computed(()=>{
  let value = 0 
  questions.value.map(q => { 
    if (q.selected == q.answer){
      value++
    }
  })
  return value
})

const getCurrentQuestion = computed(()=>{
  let question = questions.value[ currentQuestion.value ]
  question.index = currentQuestion.value
  return question
})

const setAnswer = evt => {
  questions.value[currentQuestion.value].selected = evt.target.value 
  evt.target.value = null
}

const NextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1  ) {
      currentQuestion.value++
  }else{
    quizCompleted.value = true
  }
}





</script>

<template>
<main class="app">
  <h1>The Quiz</h1>
  <section class="quiz" v-if="!quizCompleted">
    <div class="quiz-info">
      <span class="question">{{ getCurrentQuestion.question }}</span>
      <span class="score">Score {{ score }}/{{ questions.length }}</span>
    </div>
    <div class="option">
      <label v-for="(option, index) in getCurrentQuestion.options"
      :key="index"
      :class="`option ${
        getCurrentQuestion.selected == index
        ? index == getCurrentQuestion.answer
            ? 'correct'
            : 'wrong'
            : ''
      } ${
        getCurrentQuestion.selected != null && 
        index != getCurrentQuestion.selected
        ? 'disabled'
        : ''
      }`">
      <input
       type="radio"
       :name="getCurrentQuestion.index"
       :value="index"
       v-model="getCurrentQuestion.selected"
       :disabled="getCurrentQuestion.selected"
       @change="setAnswer">
       <span>{{ option }}</span>
      </label>
    </div>
    <button
    @click="NextQuestion"
    :disabled="!getCurrentQuestion.selected">
    {{ 
      getCurrentQuestion.index == questions.lenght - 1
      ? 'Finish'
      : getCurrentQuestion.selected == null
          ? 'Select an option'
          :  'Next Question'
    }}
  </button>
 
  </section>
  <section v-else>
    <h2>You have finished the quiz !</h2>
    <p> your score is {{ score }} / {{ questions.length }}</p> 
  </section>
</main>


</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Montserrat', sans-serif;
}

body {
  background-color: #0B0B0D;
  color: #F2F2F2;
}

.app {
  width: 800px;
  margin: 0 auto;
  text-align: center;
}

h1 {
  font-size: 3rem;
  margin-bottom: 2rem;
  color: #222226;
}

.quiz {
  padding: 2rem;
  border: 1px solid #F2F2F2;
  border-radius: 10px;
}

.quiz-info {
  margin-bottom: 2rem;
}

.question {
  font-size: 1.5rem;
  margin-bottom: 1rem;
  color: #F2AB27;
}

.score {
  font-size: 1rem;
  color: #BF8924;
}

.option {
  margin-bottom: 1rem;
}

.option label {
  display: block;
  padding: 0.5rem;
  border: 1px solid #F2F2F2;
  border-radius: 5px;
  cursor: pointer;
}

.option label.correct {
  background-color: #27ae60;
}

.option label.wrong {
  background-color: #f94d4d;
}

.option label.disabled {
  pointer-events: none;
}

input[type="radio"] {
  margin-right: 1rem;
}

button {
  padding: 1rem 2rem;
  border: none;
  border-radius: 10px;
  background-color: #0B0B0D;
  color: #F2F2F2;
  cursor: pointer;
}

button:hover {
  background-color: #222226;
}

.results {
  text-align: center;
  padding: 2rem;
  border: 1px solid #F2F2F2;
  border-radius: 10px;
}

h2 {
  font-size: 2rem;
  margin-bottom: 2rem;
  color: #222226;
}

p {
  font-size: 1.5rem;  
  color: #F2AB27;
}


</style>
