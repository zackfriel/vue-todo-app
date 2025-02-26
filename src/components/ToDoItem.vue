<script setup lang="ts">
const props = defineProps<{
  item: {
    key: number
    title: string
    checked: boolean
  }
  itemList: Array<{ key: number; title: string; checked: boolean }>
}>()

const { itemList } = props

console.log('item list', itemList)

const updateLocalStorage = (itemList: any) => {
  localStorage.setItem('toDoList', JSON.stringify(itemList))
}

const updateItem = (itemKey: number, itemTitle?: string, itemChecked?: boolean) => {
  const updatedList = itemList.map((item: any) => {
    if (item.key === itemKey) {
      if (itemTitle) {
        return { ...item, title: itemTitle, checked: itemChecked }
      }
      return { ...item, checked: itemChecked }
    }
    return item
  })
  updateLocalStorage(updatedList)
}

const toggleCheck = (event: Event) => {
  const checkbox = event.target as HTMLElement
  const toDoItem = checkbox.closest('.to-do-item')
  const title = toDoItem?.querySelector('.title') as HTMLInputElement
  const key = (toDoItem as HTMLElement)?.dataset.key
  if (toDoItem) {
    toDoItem.classList.toggle('checked')
    updateItem(Number(key), title.value, toDoItem.classList.contains('checked'))
  }
}

const deleteItem = (event: Event) => {
  const toDoItem = event.target as HTMLElement
  const key = (toDoItem.closest('.to-do-item') as HTMLElement)?.dataset.key
  const updatedList = itemList.filter((item: any) => item.key !== Number(key))
  updateLocalStorage(updatedList)
}

const editItem = (event: Event) => {
  const item = event.target as HTMLElement
  const parentItem = item?.closest('.to-do-item')
  parentItem?.classList.toggle('editing')
  parentItem?.querySelector('.title')?.toggleAttribute('readonly')
}

const saveItem = (event: Event) => {
  const item = event.target as HTMLElement
  const parentItem = item?.closest('.to-do-item')
  const key = (parentItem as HTMLElement)?.dataset.key
  const title = parentItem?.querySelector('.title') as HTMLInputElement
  updateItem(Number(key), title.value, parentItem?.classList.contains('checked'))
  parentItem?.classList.toggle('editing')
  title.toggleAttribute('readonly')
}
</script>

<template>
  <div class="to-do-item" :class="{ checked: item.checked }" :data-key="item.key">
    <div class="to-do-item__item">
      <button class="checkbox" v-on:click="toggleCheck"><span>✔️</span></button>
      <input class="title" readonly :value="item.title" />
    </div>
    <div class="to-do-item__tools">
      <button class="edit" v-on:click="editItem"><span>✏️</span></button>
      <button class="save" v-on:click="saveItem"><span>👍</span></button>
      <button class="delete" v-on:click="deleteItem"><span>❌</span></button>
    </div>
  </div>
</template>

<style scoped>
.to-do-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 8px;
  padding-block: 12px;
  border-top: 1px solid #ccc;
  min-width: 300px;
}

.to-do-item:first-of-type {
  border: 0;
}

.to-do-item__item,
.to-do-item__tools {
  display: flex;
  gap: 8px;
  justify-content: flex-start;
  align-items: center;
}

.to-do-item__item {
  width: calc(100% - 88px);
}

.to-do-item__tools {
  width: calc(88px + 16px);
}

.to-do-item__tools button span {
  transform: scale(1);
  transition: transform 0.3s;
}

.to-do-item__tools button:hover span {
  transform: scale(1.3);
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

.checkbox span {
  position: absolute;
  top: 50%;
  left: -50px;
  transform: translate(0, -50%);
  transition-property: top transform;
  transition-duration: 0.3s;
}

.checkbox:hover span,
.checked .checkbox span {
  left: 50%;
  transform: translate(-50%, -50%);
}

.title {
  font-size: 2rem;
  text-align: left;
  text-decoration: none;
  width: 100%;
}

[readonly].title {
  color: var(--color-text);
  background: transparent;
  border: 0;
}

.checked [readonly].title {
  text-decoration: line-through;
}

.save {
  display: none;
}

.editing .save {
  display: block;
}

.editing .edit {
  display: none;
}
</style>
