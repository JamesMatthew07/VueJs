<template>
  <HeaderComponent />
  <div class="container"></div>
  <IncomeComponent />
  <BalanceComponent :total="total" />
  <TransactionListComponent :transactions="transactions" />
  <AddTransactionComponent />
</template>

<script setup lang="ts">
import AddTransactionComponent from './components/AddTransaction.vue'
import BalanceComponent from './components/BalanceKo.vue'
import HeaderComponent from './components/HeaderKo.vue'
import IncomeComponent from './components/IncomeExpenses.vue'
import TransactionListComponent from './components/TransactionList.vue'

import { computed, ref } from 'vue'

const transactions = ref([
  { id: 1, text: 'baboy', amount: 200 },
  { id: 2, text: 'manok', amount: 100 },
  { id: 3, text: 'isda', amount: -50 },
])

const total = computed(() => {
  return transactions.value.reduce((sum, transaction) => {
    return sum + transaction.amount
  }, 0)
})

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((sum, transaction) => {
      return sum + transaction.amount
    }, 0)
    .toFixed(2)
})
</script>
