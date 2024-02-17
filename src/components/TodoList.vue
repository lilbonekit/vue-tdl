<template>
  <section class="todos">
    <SearchPanel 
      v-if="todoItems.length !== 0" 
      @query-to-parent="handleQueryUpdate"
      @filter-to-parent="handleFilterUpdate"
    />
    <ul class="list" v-if="todoItems.length !== 0 && renderFilteredTodos.length !== 0">
      <li v-for="{ id, title, descr, isDone } in renderFilteredTodos" :key="id">
        <TodoItem 
          :title="title" 
          :descr="descr" 
          :isDone="isDone" 
          :id="id" 
          @text-to-parent="handleItemEdit"
          @status-to-parent="handleItemEdit" @id-to-parent="handleItemDelete" />
      </li>
    </ul>
    <h2 v-else>
      {{ todoItems.length === 0 ? "Add your first todo:" : "No results" }}
    </h2>
    <AddPanel @inputs-to-parent="addToDo" />
  </section>
</template>

<script>
import TodoItem from './TodoItem.vue';
import AddPanel from './AddPanel.vue';
import SearchPanel from './SearchPanel.vue';

import { v4 as uuidv4 } from 'uuid';

export default {
  name: 'todo-list',
  components: {
    TodoItem,
    AddPanel,
    SearchPanel
  },
  data() {
    return {
      todoItems: [],
      currentQuery: '',
      currentFilter: null
    };
  },
  mounted() {
    // достаем из LS
    const savedTodoItems = localStorage.getItem('todoItems');

    if (savedTodoItems) {
      this.todoItems = JSON.parse(savedTodoItems);
    }
  },
  methods: {
    // Вынесли повторяющийся функционал в метод
    updateLocalStorage() {
      localStorage.setItem('todoItems', JSON.stringify(this.todoItems));
    },
    addToDo({ title, descr }) {
      const newToDo = {
        title,
        descr,
        id: uuidv4()
      }
      this.todoItems = [...this.todoItems, newToDo]
      // обновили ЛС
      this.updateLocalStorage();
    },
    updateTitleAndDescr(newTitle, newDescr, index) {
      this.todoItems = this.todoItems.map((item, i) => (i === index ? { ...item, title: newTitle, descr: newDescr } : item));
      // Обновили ЛС
      this.updateLocalStorage();
    },
    updateStatus(newStatus, index) {
      this.todoItems = this.todoItems.map((item, i) => (i === index ? { ...item, isDone: newStatus } : item));
      // Обновили ЛС
      this.updateLocalStorage();
    },
    handleItemEdit(data) {
      const { id } = data.payload
      const index = this.todoItems.findIndex(item => item.id === id);

      if (index === -1) return alert('Error')

      switch (data.type) {
        // пейлод где нам пришел статус
        case 'status': {
          const { isDone } = data.payload
          this.updateStatus(isDone, index)
          break;
        }

        case 'text': {
          // пейлод где пришли текстовые изменения
          const { title, descr } = data.payload
          this.updateTitleAndDescr(title, descr, index)
          break;
        }
      }

    },
    handleItemDelete(id) {
      this.todoItems = this.todoItems.filter(todo => todo.id !== id);
      // удалили - обновили ЛС
      this.updateLocalStorage();

    },
    handleFilterUpdate(filter) {
      this.currentFilter = filter
    },
    handleQueryUpdate(query) {
      this.currentQuery = query
    }
  },
  computed: {
  renderFilteredTodos() {
    if (this.currentFilter === null) {
      return this.todoItems
        .filter(todo => 
          todo.title.toLowerCase().includes(this.currentQuery.toLowerCase()) ||
          todo.descr.toLowerCase().includes(this.currentQuery.toLowerCase())
        )
        .reverse();
    } else {
      return this.todoItems
        .filter(todo => 
          todo.isDone === this.currentFilter &&
          (todo.title.toLowerCase().includes(this.currentQuery.toLowerCase()) ||
           todo.descr.toLowerCase().includes(this.currentQuery.toLowerCase()))
        )
        .reverse();
    }
    }
  }
}
</script>

<style lang="scss" scoped>
.todos {
  display: flex;
  justify-content: space-between;
  gap: 30px;
  flex-wrap: wrap;
  h2 {
    display: flex;
    flex-grow: 5;
  }
}

.list {
  list-style-type: none;
  display: flex;
  flex-grow: 3;
  flex-direction: column;
  gap: 10px;
  max-height: 500px;
  overflow-y: auto;
}

ul {
  padding-left: 0;
  border: 1px solid #ccc;
  border-radius: 20px;
  padding: 10px;
  margin: 0;

  li {
    display: flex;
  }
  
}
</style>