<template>
  <h3>History List</h3>
  <ul class="list" id="list">
    <li
      v-for="(transaction, index) in transactions"
      :key="index"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }} <span>${{ transaction.amount }}</span>
      <button @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
    </li>
  </ul>
</template>

<script setup>
import { defineProps } from 'vue'
const emit = defineEmits(['transactionDeleted'])
defineProps({
  transactions: {
    type: Array,
    required: true
  }
})

const deleteTransaction = (id) => {
  emit('transactionDeleted', id)
}
</script>
