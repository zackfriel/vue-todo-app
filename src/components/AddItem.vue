<script setup lang="ts">
const props = defineProps<{
  itemList: Array<{ key: number; title: string; checked: boolean }>
}>()

const { itemList } = props

const addItem = () => {
  const toDoList = itemList
  const newToDo = {
    key: toDoList.length + 1,
    title: 'New To Do',
    checked: false,
  }
  toDoList.push(newToDo)
  localStorage.setItem('toDoList', JSON.stringify(toDoList))
}
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
