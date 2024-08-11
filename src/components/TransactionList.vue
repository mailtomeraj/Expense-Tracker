<template>
    <h3>History</h3>
    
    <ul id="list" class="list">
      <li  v-for="transaction in props.transactions" :key="transaction.id" :class="transaction.amount < 0 ? 'minus' : 'plus' " >
      {{transaction.text}}<span>{{transaction.amount}}</span>
      <button class="delete-btn" @click="transactionDelete(transaction.id)" >x</button>
      </li>  
    </ul>
    <div v-if="transactions.length == 0 ">No data Available</div>
</template>

<script setup>
 import { defineProps, defineEmits } from 'vue';
 const emit = defineEmits(['transactionDeleted'])
 const props = defineProps(['transactions'])

 const transactionDelete = (id) => {
  emit('transactionDeleted', id)
 }

</script>

<style  scoped>
.red{
  color:red
}
</style>