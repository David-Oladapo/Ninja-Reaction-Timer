<template>
    <!-- only shows if showBlock is true and on click calls the stopTimer -->
    <div class="block" :style="popupStyle" v-if="showBlock" @click="stopTimer" >
        click me!
    </div>
</template>

<script setup>
        // imports various resources from vue library 
import { defineProps, onMounted, ref, defineEmits } from 'vue'

    const showBlock = ref(false)
    const timer = ref(null)
    const reactionTime = ref(0)
    const popupStyle = ref({
        position: 'fixed',
        top: '0px',
        left: '0px',
    })
    
    function generateRandomPosition() {
        const x = Math.floor(Math.random() * (window.innerWidth - 250))
        const y = Math.floor(Math.random() * (window.innerHeight - 150))
            popupStyle.value.top = `${y}px`
            popupStyle.value.left = `${x}px`
    }

            // collects the props from the parent component and stores in "props" using the defineProps function in vue
    const  props = defineProps({
        delay: Number,
    })

    const toggleShow = () => {
        showBlock.value = true
    }

            // sets a time interval in which the value of "reactionTime" keeps increasing by 10 every 10 milliseconds
            // and stores the interval NOT the actual "reactionTime" value in "timer"
    const startTimer = () => {
       timer.value = setInterval(() =>{
        reactionTime.value += 10
       } , 10)
    }

            // stops the time interval and clears it and then calls the "handleEmit" function
    const stopTimer = () => {
        console.log(reactionTime.value);
        clearInterval(timer.value)
        handleEmit()
    }

            // defines the custom event "end" to be emitted by using defineEmits in vue
    const emit = defineEmits(['end'])

            // this handles the emitting of the custom event to the parent component
            // and also passes data along with it which is the reactionTime value after the time interval has stopped
    const handleEmit = () => {
        emit('end' ,reactionTime.value)
    }

            // when the block component is mounted to the DOM it sets a timer using the delay value as the time  
            // in which the two functions "toggleShow" and "startTimer" are then called
    onMounted(() => {
            generateRandomPosition()
            console.log(popupStyle.value)
            console.log('component mounted')
            setTimeout(toggleShow, props.delay)
            setTimeout(startTimer, props.delay)
    })

</script>

<style>
.block {
    width: 250px;
    height: 150px;
    margin: 100px auto;
    background-color: lightgreen;
    color: black;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 10px;
}
</style>