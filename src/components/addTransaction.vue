<template>
  <h3>Add New Transaction</h3>

  <form id="id" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" v-model="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >amount <br />
        (negative = expense / positive = income)</label
      >
      <input type="text" id="amount" v-model="amount" placeholder="Enter amount..." />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref } from 'vue'
import { useToast } from 'vue-toastification'
const toast = useToast()

const text = ref('')
const amount = ref('')
const emit = defineEmits(['transactionsSubmitted'])
const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error('both fields must be filled')
    return
  }

  const transactionsData = {
    text: text.value,
    amount: parseFloat(amount.value)
  }

  text.value = ''
  amount.value = ''

  emit('transactionsSubmitted', transactionsData)
}
</script>
