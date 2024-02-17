<template>
  <div>
    <ul class="list">
      <li
        v-for="{ id, title, descr, isDone } in todoItems"  
        :key="id" 
      >
        <TodoItem 
          :title="title" 
          :descr="descr"
          :isDone="isDone"
          @edit="handleItemEdit"
        />
      </li>
    </ul>
  </div>
</template>

<script>
import TodoItem from './TodoItem.vue';

export default {
  name: 'todo-list',
  components: {
    TodoItem
  },
  data() {
    return {
      todoItems: [
        { id: 1, title: 'First Item', descr: 'Description 1', isDone: false },
        { id: 2, title: 'Second Item', descr: 'Description 2', isDone: true },
      ]
    };
  }
}
</script>

<style lang="scss" scoped>
  .list {
    list-style-type: none;
    display: flex;
    flex-direction: column;
    gap: 20px;
  }
</style>