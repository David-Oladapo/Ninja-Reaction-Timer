<template>
    <!-- displays the reaction time value which was stored in the "score" prop -->
    <p>Reaction Time: {{ props.score }}ms</p>
    <h4 class="rank">{{ rank }}</h4>
</template>

<script setup>
import { defineProps, onMounted, ref } from 'vue';
const rank = ref(null)
const props = defineProps ({
    score: Number
})

// conditional logic to display a specific rank message in the template based on the score value
onMounted(() => {
    if(props.score <= 250){
        rank.value = 'Ninja Fingers'
    }else if(props.score <= 400){
        rank.value = 'Rapid reflexes'
    }else{
        rank.value = 'Snail pace...'
    }
})
</script>

<style scoped>

p{
    text-align: center;
    margin: 100px auto 20px;
}

.rank{
    text-align: center;
    font-size: 1.4rem;
    color: lightgreen;
    font-weight: bold;
}
</style>