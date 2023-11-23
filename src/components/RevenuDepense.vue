<template>
    <div class="inc-exp-container">
        <div>
            <h4>Revenu</h4>
            <p id="money-plus" class="money plus">+{{ revenu }} €</p>
        </div>
        <div>
            <h4>Dépense</h4>
            <p id="money-minus" class="money minus">{{ depense }} €</p>
        </div>
    </div>
</template>

<script setup>
import {defineProps} from 'vue';

const props = defineProps({
    revenu: {
        type: Number,
        required: true,
    },
    depense: {
        type: Number,
        required: true,
    },
});
</script>

<style scoped>

</style>