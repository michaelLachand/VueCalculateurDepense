<template>
    <Header/>
    <div class="container">
        <Solde :total="total"/>
        <RevenuDepense :revenu="revenu" :depense="depense"/>
        <ListeTransaction :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
        <AjoutTransaction @transactionSubmitted="handleTransactionSubmitted"/>
    </div>
</template>

<script setup>
import Header from "@/components/Header.vue";
import Solde from "@/components/Solde.vue";
import RevenuDepense from "@/components/RevenuDepense.vue";
import ListeTransaction from "@/components/ListeTransaction.vue";
import AjoutTransaction from "@/components/AjoutTransaction.vue";

import {useToast} from "vue-toastification";

import {ref, computed, onMounted} from "vue";

const toast = useToast();

const transactions = ref([]);

onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

    if (savedTransactions) {
        transactions.value = savedTransactions;
    }
})

const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
        return acc + transaction.montant;
    }, 0);
})

const revenu = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.montant > 0)
        .reduce((acc, transaction) => {
            return acc + transaction.montant;
        }, 0)
        .toFixed(2);
})

const depense = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.montant < 0)
        .reduce((acc, transaction) => {
            return acc + transaction.montant;
        }, 0)
        .toFixed(2);
})

const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
        id: generateUniqueId(),
        text: transactionData.text,
        montant: transactionData.montant,
    });

    saveTransactionsToLocalStorage();

    toast.success('Transaction ajoutée');
};

const generateUniqueId = () => {
    return Math.floor(Math.random() * 100000);
}

const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter(
        (transaction) => transaction.id !== id);

    saveTransactionsToLocalStorage();

    toast.success('Transaction supprimée');
}

const saveTransactionsToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
}
</script>