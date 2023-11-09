<template>
    <h3>Add new transaction</h3>
    <form id="form" @submit.prevent="onsubmit">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" id="text" placeholder="Enter text..." v-model="text" />
        </div>
        <div class="form-control">
            <label for="amount">Amount <br />
                (negative - expense, positive - income)</label>
            <input type="number" v-model="amount" id="amount" placeholder="Enter amount..." />
        </div>
        <button class="btn">Add transaction</button>
    </form>
</template>
<script setup>
import { ref, useTransitionState } from 'vue';
import { useToast } from 'vue-toastification'
const toast = useToast()
const text = ref('')
const amount = ref('')
const emit = defineEmits(['transactionSubmitted'])
const onsubmit = () => {
    if (!text.value || !amount.value) {
        toast.error('both fileds must be filled')
        return
    }
    const transactionData = {
        text: text.value,
        amount: parseFloat(amount.value)

    }
    emit('transactionSubmitted', transactionData)

    text.value = ''
    amount.value = ''
}
</script>
