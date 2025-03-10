<script setup lang="ts">
import { ref, computed, watch } from 'vue'
import ToDoItem from '@/components/ToDoItem.vue'
import AddItem from '@/components/AddItem.vue'

const dataFromLocalStorage = localStorage.getItem('toDoList')
if (!dataFromLocalStorage) {
  localStorage.setItem('toDoList', JSON.stringify([]))
}
const list = ref(dataFromLocalStorage ? JSON.parse(dataFromLocalStorage) : [])
const itemList = computed(() => list.value)

watch(itemList, (newList) => {
  localStorage.setItem('toDoList', JSON.stringify(newList))
  console.log('List updated:', newList)
})

const toggleCheck = (event: { target: any }) => {
  const checkbox = event.target as HTMLElement
  const toDoItem = checkbox.closest('.to-do-item') as HTMLElement
  const title = toDoItem?.querySelector('.title') as HTMLInputElement
  const key = toDoItem?.dataset.key
  if (toDoItem) {
    toDoItem.classList.toggle('checked')
    updateItem(Number(key), title.value, toDoItem.classList.contains('checked'))
  }
}

const deleteItem = (event: Event) => {
  const item = event.target as HTMLElement
  const parentItem = item.closest('.to-do-item')
  const key = (parentItem as HTMLElement)?.dataset.key
  list.value = list.value.filter((item: { key: number }) => item.key !== Number(key))
}

const updateItem = (itemKey: number, itemTitle: string, itemChecked: boolean) => {
  list.value = list.value.map((item: { key: number; title: string; checked: boolean }) => {
    if (item.key === itemKey) {
      return { ...item, title: itemTitle, checked: itemChecked }
    }
    return item
  })
}

const editItem = (event: Event) => {
  const item = event.target as HTMLElement
  const parentItem = item.closest('.to-do-item')
  parentItem?.classList.toggle('editing')
  parentItem?.querySelector('.title')?.toggleAttribute('readonly')
}

const saveItem = (event: Event) => {
  const item = event.target as HTMLElement
  const parentItem = item.closest('.to-do-item')
  const key = (parentItem as HTMLElement)?.dataset.key
  const title = parentItem?.querySelector('.title') as HTMLInputElement
  updateItem(Number(key), title.value, !!parentItem?.classList.contains('checked'))
  parentItem?.classList.toggle('editing')
  title.toggleAttribute('readonly')
}

const addItem = () => {
  const toDoList = list.value
  const newToDo = {
    key: toDoList.length + 1,
    title: 'New To Do',
    checked: false,
  }
  toDoList.push(newToDo)
  console.log('New item added:', newToDo)
  localStorage.setItem('toDoList', JSON.stringify(toDoList))
  list.value = toDoList
  console.log('List updated:', list.value)
}
</script>

<template>
  <h1>To Do App</h1>
  <div class="to-do-list">
    <ToDoItem
      v-if="itemList"
      v-for="(item, index) in itemList"
      :key="index"
      :item="item"
      :itemList="itemList"
      :deleteItem="deleteItem"
      :toggleCheck="toggleCheck"
      :editItem="editItem"
      :saveItem="saveItem"
    />
    <AddItem :itemList="itemList" :addItem="addItem" />
  </div>
</template>

<style scoped>
h1 {
  font-size: 3rem;
}

.to-do-list {
  min-height: 300px;
  width: 100%;
}
</style>
