<template>
    <h3>Historique</h3>
    <ul id="list" class="list">
        <li
                v-for="transaction in transactions"
                :key="transaction.id"
                :class="transaction.montant < 0 ? 'minus' : 'plus'"
        >
            {{ transaction.text }} <span>
      {{ transaction.montant }} € </span>
            <button @click="deleteTransaction(transaction.id)" class="delete-btn">X</button>
        </li>
    </ul>
</template>

<script setup>
import {defineProps} from 'vue';

const emit = defineEmits(['transactionDeleted']);

const props = defineProps({
    transactions: {
        type: Array,
        required: true,
    },
});

const deleteTransaction = (id) => {
    emit('transactionDeleted', id);
}
</script>
