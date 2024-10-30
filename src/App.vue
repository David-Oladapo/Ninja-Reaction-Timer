<!-- when the click event in the button is activated, it calls the function "start" and also the button is disabled when the value of "isPlaying" is true -->
 <!-- the child components "Block" and "Results" are then called based on the conditions placed in v-if -->
  <!-- data is bind to the props that are passed from this component to the child components, "delay" to "Block" and "score" to "Results"  -->
   <!-- in the Block component when the custom event "end" is activated, it calls the endGame function -->

<template>
  <header>Ninja Reaction timer</header>
  <br/>
  <button v-if="showResult === true" @click="start" :disabled="isPlaying" class="start-btn">Play Again</button>
  <button v-else @click="start" :disabled="isPlaying" class="start-btn">Play Game</button>
    <Block v-if="isPlaying" :delay="delay" @end="endGame"/>
    <Results v-if="showResult" :score="score"/>
</template>


<script setup>
//NB: This whole section was written in composition api syntax using the <script setup> block

        //importing the various resources and child components
import Results from './components/Results.vue'
import Block from './components/Block.vue'
import { ref } from 'vue'

        // setting the following data to null and false initially, which would be changed later 
        // was able to do this because the constant variables are referencing the ref function from vue which holds the values
        // not necessarily the constant variables themselves 
  const isPlaying = ref(false)
  const delay = ref(null)
  const score = ref(null)
  const showResult = ref(null)


        // called when button is clicked 
        // changes isPlaying value to true, gives delay a random value between 2000 and 7000 using the Math.Random() function 
        // and logs the value to the console
  const start = () => {
    isPlaying.value = true
    delay.value = 2000 + Math.random() * 5000
    console.log(delay.value)
    showResult.value = false
  }


        // is called when the custom event is emitted from the Block component and the data attached to the event -reactionTime
        // which is then stored in the constant variable "score"
        // changes the isPlaying back to false signifying that the game has ended and a new game can now be played
        // changes showResult to true which then validates the condition set in the v-if in line 7
  const endGame = (reactionTime) => {
    score.value = reactionTime
    isPlaying.value = false
    showResult.value = true
  }
</script>


<style scoped>
header {
  line-height: 1.5;
  font-size: 2rem;
  font-weight: bold;
}

.start-btn {
  display: flex;
  place-self: center;
  background-color: lightgreen;
  cursor: pointer;
  padding: 8px 16px;
  border-style: none;
  border-radius: 4px;
  font-size: 16px;
  letter-spacing: 1px;
  margin: 10px auto;
}

.start-btn[disabled] {
  opacity: 0.2;
  cursor: not-allowed;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    /* padding-right: calc(var(--section-gap) / 2); */
  }

  .logo {
    margin: 0 2rem 0 0;
  }
}
</style>
