<template>
  <Header />
  <div class="container">
    <StartBalance @add-transaction="handleTransactionSubmitted" />
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
  import Header from './components/Header.vue';
  import StartBalance from './components/StartBalance.vue'
  import Balance from './components/Balance.vue';
  import IncomeExpenses from './components/IncomeExpenses.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';

  import {useToast} from 'vue-toastification';

  import { ref, computed, onMounted } from 'vue';

    const toast = useToast();

    const transactions = ref([]);

    onMounted(() => {
      const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

      if(savedTransactions) {
        transactions.value = savedTransactions;
      }
    });

    //Get Total
    const total = computed(() => {
      return transactions.value.reduce((acc, transaction) => {
        return acc + transaction.amount;
      }, 0);
    });
    
    //Get Income
    const income = computed(() => {
      return transactions.value
      .filter((transaction) => transaction.amount > 0)
      .reduce((acc, transaction) => {
        return acc + transaction.amount;
      }, 0)
      .toFixed(2);
    });

    //Get Expenses
    const expenses = computed(() => {
      return transactions.value
      .filter((transaction) => transaction.amount < 0)
      .reduce((acc, transaction) => {
        return acc + transaction.amount;
      }, 0)
      .toFixed(2);
    });

    //Add Transaction
    const handleTransactionSubmitted = (transactionData) => {
      transactions.value.push({
        id: generateUniqueId(),
        text: transactionData.text,
        amount: transactionData.amount
      });

      saveTransactionsToLocalStorage();

      toast.success('İşlem Başarıyla Yapıldı');
    };

    //Generate Unique ID
    const generateUniqueId = () => {
      return Math.floor(Math.random() * 1000000);
    };

    //Delete Transaction
    const handleTransactionDeleted = (id) => {
      transactions.value = transactions.value.filter((transaction) => transaction.id !== id);

      saveTransactionsToLocalStorage();

      toast.success('İşlem Başarıyla Silindi');
    };

    //Save to localstorage
    const saveTransactionsToLocalStorage = () => {
      localStorage.setItem('transactions', JSON.stringify(transactions.value));
    };

    //Start Balance
    const handleStartBalance = (balance) => {
      startBalance.value = balance;
      
      saveTransactionsToLocalStorage();

      toast.success('Başlangıç Bakiyesi Eklendi');
    };
</script>