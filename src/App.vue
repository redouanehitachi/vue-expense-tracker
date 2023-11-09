<template>
  <div>
    <Header />
    <div class="container">
      <Balance :total="total" />
    </div>
    <IncomeExpenses :income="income" :expense="expense" />
    <transaction :transactions="transactions" @deleteTransaction="handleDeleteTransaction" />
    <AddTransaction @transactionSubmitted="handleTransactionDeleted" />
  </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import transaction from './components/transaction.vue';
import IncomeExpenses from './components/IncomeExpenses.vue'
import AddTransaction from './components/AddTransaction.vue';
import { ref, computed, onMounted } from 'vue';
import { useToast } from 'vue-toastification';
const toast = useToast()
onMounted(() => {
  const savedTansactions = JSON.parse(localStorage.getItem('transactions'))
  if (savedTansactions) {
    transactions.value = savedTansactions

  }
})
const transactions = ref([
  { id: 1, text: 'Klein', amount: -5.0135 },
  { id: 2, text: 'Chandler', amount: -10.8574 },
  { id: 3, text: 'Watts', amount: 88.249 },
  { id: 4, text: 'Obrien', amount: 4.7567 },
])
const total = computed(() => {
  return transactions.value.reduce((acc, tra) => {
    return acc + tra.amount
  }, 0)
})
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0).toFixed(2)
})
const expense = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0).toFixed(2)
})
const handleTransaction = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    amount: transactionData.amount,
    text: transactionData.text
  })
  toast.success('transaction added')
}
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000)
}

const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );

  saveTransactionsToLocalStorage();

  toast.success('Transaction deleted.');
};
</script>
