<script setup>

// eslint-disable-next-line @typescript-eslint/no-unused-vars
const props = defineProps({
    msg: String
})

const emit = defineEmits(['response'])

emit('response', 'hello from child')
</script>

<template>
    <h2>Un componente figlio!</h2>
    <h2>{{ msg || 'Ancora nessuna props passata' }}</h2>
    <slot>Contenuto di fallback</slot>
</template>