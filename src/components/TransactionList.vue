<template>
  <div>
    <h3>History</h3>
    <ul id="list" class="list">
      <!-- <li class="minus">
        Cash <span>-$400</span>
        <button class="delete-btn">x</button>
      </li>
      <li class="plus">
        Paycheck <span>$100</span>
        <button class="delete-btn">x</button>
      </li> -->
      <li
        v-for="transaction in props.transactions"
        v-bind:key="transaction.id"
        :class="transaction.amount < 0 ? 'minus' : 'plus'"
      >
        {{ transaction.text }} <span>{{ transaction.amount }}</span>
        <button @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { defineProps } from 'vue'

const emit = defineEmits(['transactionDeleted']);

const props = defineProps ({
  transactions: {
    type: Array,
    required: true,
  }
})

const deleteTransaction = (id) =>
{
  emit('transactionDeleted', id)
}
</script>
