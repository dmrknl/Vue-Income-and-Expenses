<template>
    <div class="start-balance">
      <input
        type="number"
        v-model.number="startBalance"
        placeholder="Başlangıç Bakiyesi Girin"
      />
      <button @click="emitStartBalance">Başlangıç Bakiyesi Ekle</button>
    </div>
  </template>
  
  <script>
  export default {
    name: "StartBalance",
    data() {
      return {
        startBalance: null,
      };
    },
    methods: {
      emitStartBalance() {
        if (this.startBalance === null || this.startBalance === 0) return;
        this.$emit("add-transaction", {
          id: Math.floor(Math.random() * 100000000),
          text: "Başlangıç Bakiyesi",
          amount: +this.startBalance,
        });
  
        this.$emit("start-balance-set", this.startBalance);
        this.startBalance = null;
      },
    },
  };
  </script>  