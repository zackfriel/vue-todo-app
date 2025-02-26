<script lang="ts">
import { ref, computed, watch } from 'vue'
import ToDoItem from '@/components/ToDoItem.vue'
import AddItem from '@/components/AddItem.vue'

const dataFromLocalStorage = localStorage.getItem('toDoList')
if (!dataFromLocalStorage) {
  localStorage.setItem('toDoList', JSON.stringify([]))
}
const list = ref(dataFromLocalStorage ? JSON.parse(dataFromLocalStorage) : [])
const itemList = computed(() => list.value)

export default {
  name: 'ToDoList',
  components: {
    ToDoItem,
    AddItem,
  },
  data() {
    return {
      itemList,
    }
  },
  methods: {
    updateList(newList: any) {
      this.itemList = newList
    },
  },
  watch: {
    itemList(newList: any) {
      localStorage.setItem('toDoList', JSON.stringify(newList))
    },
  },
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
      @update-data="updateList"
    />
    <AddItem :itemList="itemList" />
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
