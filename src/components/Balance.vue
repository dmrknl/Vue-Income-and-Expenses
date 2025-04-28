<template>
    <h4>Bakiyeniz</h4>
    <h1 id="balance">₺{{ total }}</h1>
</template>

<script setup>
    import {defineProps} from 'vue';

    const props = defineProps({
        total: {
            type: Number,
            required: true,
        },
    });
</script>