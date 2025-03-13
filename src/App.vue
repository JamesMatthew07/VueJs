<template>
  <HeaderComponent />
  <div class="container"></div>
  <IncomeComponent :income="income" :expenses = "expenses"/>
  <BalanceComponent :total="total" />
  <TransactionListComponent :transactions="transactions" @transactionDeleted = "handleTransactionDeleted" />
  <AddTransactionComponent @transactionCompleted="handleTransactionCompleted" />
</template>

<script setup lang="ts">
import AddTransactionComponent from './components/AddTransaction.vue'
import BalanceComponent from './components/BalanceKo.vue'
import HeaderComponent from './components/HeaderKo.vue'
import IncomeComponent from './components/IncomeExpenses.vue'
import TransactionListComponent from './components/TransactionList.vue'
import { useToast } from 'vue-toastification'

import { computed, ref, onMounted } from 'vue'

const toast = useToast();

const transactions = ref([
  
  // { id: 1, text: 'baboy', amount: 200 },
  // { id: 2, text: 'manok', amount: -100 },
  // { id: 3, text: 'isda', amount: -50 }
])

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem ('transactions'));

  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

const savedTransactionsToLocalStorage = () => {
    localStorage.setItem('transaction', JSON.stringify(transactions.value))
  }

const total = computed(() => {
  return transactions.value.reduce((comp, transaction) => {
    return comp + transaction.amount
  }, 0)
})

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((comp, transaction) => {
      return comp + transaction.amount
    }, 0)
    .toFixed(2)
})

const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((comp, transaction) => {
      return comp + transaction.amount }
      ,0)
      .toFixed(2)
    })

  const handleTransactionCompleted = (transactionData) => {
    transactions.value.push({
      id: generateUniqueId(),
      text: transactionData.text,
      amount: transactionData.amount
    })

    savedTransactionsToLocalStorage()

    // After calling savedTransactionsToLocalStorage, you can check the localStorage in the browser console:
localStorage.getItem('transactions')


    toast.success("Transaction Added")
  }

  const generateUniqueId = () => {
    return Math.floor(Math.random() * 10000);
  }


  const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter ((transaction) => transaction.id !== id)

    savedTransactionsToLocalStorage();

    toast.success('Transaction Deleted')
  }

</script>
