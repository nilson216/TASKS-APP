

<script lang="ts" setup>
import type { TaskFilter } from "../types.ts";

const props = defineProps<{
    filter: TaskFilter;
}>();

const emits = defineEmits<{
    setFilter: [filter: TaskFilter];
}>();
</script>

<template>
     <button class="secondary" @click="emits('setFilter', props.filter)">{{ props.filter }}</button>
</template>

<style scoped>
    button {
        margin-right: 0.5rem;
        text-transform: capitalize;
    }
</style>