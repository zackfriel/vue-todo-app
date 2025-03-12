<script setup lang="ts">
import { defineProps } from 'vue'

const props = defineProps<{
  item: {
    key: number
    title: string
    checked: boolean
  }
  itemList: Array<{ key: number; title: string; checked: boolean }>
  toggleCheck: (event: Event) => void
  deleteItem: (event: Event) => void
  editItem: (event: Event) => void
  saveItem: (event: Event) => void
}>()
</script>

<template>
  <div class="to-do-item" :class="{ checked: item.checked }" :data-key="item.key">
    <div class="to-do-item__item">
      <button class="checkbox" v-on:click="props.toggleCheck"><span>✔️</span></button>
      <input class="title" readonly :value="item.title" />
    </div>
    <div class="to-do-item__tools">
      <button class="edit" v-on:click="props.editItem"><span>✏️</span></button>
      <button class="save" v-on:click="props.saveItem"><span>👍</span></button>
      <button class="delete" v-on:click="props.deleteItem" :data-key="item.key">
        <span>❌</span>
      </button>
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
  display: flex;
}

.editing .edit {
  display: none;
}
</style>
