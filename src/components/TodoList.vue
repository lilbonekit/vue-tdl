<template>
  <div>
    <ul class="list">
      <li v-for="{ id, title, descr, isDone } in todoItems" :key="id">
        <TodoItem 
          :title="title" 
          :descr="descr" 
          :isDone="isDone" 
          :id="id" 
          @text-to-parent="handleItemEdit"
          @status-to-parent="handleItemEdit"
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
        { id: 'aswwwww', title: 'First Item', descr: 'Description 1', isDone: false },
        { id: '22d2dds', title: 'Second Item', descr: 'Description 2', isDone: true },
      ]
    };
  },
  methods: {
    updateTitleAndDescr(newTitle, newDescr, index) {
      this.todoItems = this.todoItems.map((item, i) => (i === index ? { ...item, title: newTitle, descr: newDescr } : item));
    },
    updateStatus(newStatus, index) {
      this.todoItems = this.todoItems.map((item, i) => (i === index ? { ...item, isDone: newStatus } : item));
    },
    handleItemEdit(data) {
      const {id} = data.payload
      const index = this.todoItems.findIndex(item => item.id === id);

      if(index === -1) return alert('Error')

      switch (data.type) {
        case 'status': {
          const {isDone} = data.payload
          this.updateStatus(isDone, index)
          break;
        } 
        
        case 'text': {
          const {title, descr} = data.payload
          this.updateTitleAndDescr(title, descr, index)
          break;
        } 
      }

    }
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