<script setup lang="ts">
import ToDoList from '@/components/ToDoList.vue'
</script>

<template>
  <main>
    <ToDoList />
  </main>
</template>

<style scoped>
main {
  display: flex;
  flex-direction: column;
  gap: 16px;
  min-width: calc(100dvw - calc(var(--site-padding) * 2));
  min-height: calc(100dvh - (var(--site-padding) * 2));
}
</style>
