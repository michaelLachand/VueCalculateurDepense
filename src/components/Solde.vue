<template>
    <h4>Solde</h4>
    <h1 id="solde">{{ total }} €</h1>
</template>

<script setup>
import {defineProps} from 'vue';

const props = defineProps({
    total: {
        type: Number,
        required: true,
    }
})
</script>


<style scoped>

</style>