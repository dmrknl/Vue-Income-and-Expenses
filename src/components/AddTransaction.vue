<template>
    <h3>İşlem eklemelisiniz</h3>
    <form id="form" @submit.prevent="onSubmit">
      <div class="form-control">
        <label for="text">Metin</label>
        <input type="text" id="text" v-model="text" placeholder="Metin giriniz..." />
      </div>
      <div class="form-control">
        <label for="amount"
          >Miktar <br />
          (Negatif - Gider | Pozitif - Gelir)</label>
        <input type="text" id="amount" v-model="amount" placeholder="Miktar giriniz..."/>
      </div>
      <button class="btn">Ekle</button>
    </form>
</template>

<script setup>
import {ref} from 'vue';
import {useToast} from 'vue-toastification';

const text = ref('');
const amount = ref('');
const emit = defineEmits(['transactionSubmitted']);
const toast = useToast();

const onSubmit = () => {
    if (!text.value || !amount.value) {
        toast.error('Tüm Alanlar Doldurulmalıdır!');
        return;
    }

const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value),
};

emit('transactionSubmitted', transactionData);

    text.value='';
    amount.value='';
};
</script>