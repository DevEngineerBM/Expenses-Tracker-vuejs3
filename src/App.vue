<template>
  <headerApp />
  <div class="container">
    <balanceApp :total="total" />
    <incomeExpenses :income="income" :expense="expense" />
    <historyList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
    <addTransaction @transactionsSubmitted="handleTransactionsSubmitted" />
  </div>
</template>

<script setup>
import headerApp from './components/headerApp.vue'
import balanceApp from './components/balanceApp.vue'
import incomeExpenses from './components/incomeExpenses.vue'
import historyList from './components/historyList.vue'
import addTransaction from './components/addTransaction.vue'
import { useToast } from 'vue-toastification'
const toast = useToast()

import { ref, computed, onMounted } from 'vue'

const transactions = ref([
  onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'))

    if (savedTransactions) {
      transactions.value = savedTransactions
    }
  })
])

// TOTAL CALCULATION
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount
  }, 0)
})

// INCOME CALCULATION

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
})

// EXPENSES CALCULATION

const expense = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
})

// GENERATE ID
const generateId = () => {
  return Math.floor(Math.random() * 1000000)
}
// ADD TRANSACTION
const handleTransactionsSubmitted = (transactionsData) => {
  transactions.value.push({
    id: generateId(),
    text: transactionsData.text,
    amount: transactionsData.amount
  })
  saveData()
  toast.success('Transaction Added succesfully')
}

// DELETE TRANSACTION

const handleTransactionDeleted = (id) => {
  const initialLength = transactions.value.length

  transactions.value = transactions.value.filter((transaction) => transaction.id !== id)

  saveData()

  if (initialLength > transactions.value.length) {
    toast.success('Transaction Deleted')
  } else {
    toast.error('Transaction Not Found')
  }
}

// SAVE DATA

const saveData = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script>
