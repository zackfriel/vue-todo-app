<script setup lang="ts">
const props = defineProps<{
  itemList: Array<{ key: number; title: string; checked: boolean }>
  addItem: () => void
}>()
</script>

<template>
  <button class="add" v-on:click="addItem">➕</button>
</template>

<style scoped>
.title {
  font-size: 1.5rem;
  text-align: left;
  text-decoration: none;
  width: 100px;
}

button {
  width: 44px;
  height: 44px;
  cursor: pointer;
  font-size: 1.3rem;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 8px;
  border: 0;
  overflow: hidden;
  position: relative;
}

.add:hover {
  background-color: greenyellow;
}
</style>
