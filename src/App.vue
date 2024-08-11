<script setup>
import { RouterLink, RouterView } from 'vue-router'
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransactions from './components/AddTransactions.vue';
import { computed, ref, onMounted } from 'vue';
import { useToast } from 'vue-toastification';

const transactions = ref([])
const toast = useToast()

onMounted(()=>{
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'))

  if(savedTransactions){
    transactions.value = savedTransactions
  }
})

//total
const total = computed(()=>{
  return transactions.value.reduce((acc, transaction)=>{
    return acc + transaction.amount
  }, 0).toFixed(2)
})

//income
const income = computed(()=>{
  return transactions.value
  .filter((transaction)=> transaction.amount > 0)
  .reduce((acc, transaction)=>{
    return acc + transaction.amount
  }, 0).toFixed(2)
})

//expense
const expense = computed(()=>{
  return transactions.value
  .filter((transaction)=> transaction.amount < 0)
  .reduce((acc, transaction)=>{
    return acc + transaction.amount
  }, 0).toFixed(2)
})

const handleTransactionSubmitted = (transactionData) => {
 transactions.value.push({
   id:generateUniqueId(),
   text: transactionData.text,
   amount: transactionData.amount
 })
 savedTransactionsToLocalStorage()
 toast.success("Transaction added successfully")
}

const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000)
}

const handleDelete = (id) => {
    transactions.value = transactions.value.filter((transaction) => transaction.id !== id)
    savedTransactionsToLocalStorage()
    toast.success("Transaction deleted")
}

const savedTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script>

<template>
<Header/>
<div class="container">
  <Balance :total="+total"/>
  <IncomeExpense :income="+income" :expense="-expense"/>
  <TransactionList :transactions="transactions"  @transactionDeleted="handleDelete"/>
  <AddTransactions @transactionSubmitted="handleTransactionSubmitted" />
</div>
  <RouterView />
</template>

<style scoped>


</style>
