<template>
  <h4>your balance</h4>
  <h1 class="balance">${{ total }}</h1>
</template>

<script setup>
import { defineProps } from 'vue'

defineProps({
  total: {
    type: Number,
    required: true
  }
})
</script>
