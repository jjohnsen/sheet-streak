<script setup lang="ts">
    import confetti from 'canvas-confetti';
    import { onMounted } from 'vue';

    onMounted(() => {
        confetti({
            particleCount: 500,
            spread: 300,
            origin: { y: 0.3 }
        });
    });
</script>
<template></template>
