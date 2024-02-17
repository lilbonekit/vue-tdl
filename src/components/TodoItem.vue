<template>
  <div class="list__item" :class="{ 'list__item_done': isDone }">
    <div class="list__item-wrapper">
      <input type="checkbox" v-model="isDone">
      <div class="list__item-text">
        <label>
          <h3>{{ truncatedTitle }}</h3>
          <input v-if="isEditable" type="text" v-model="titleInput" />
        </label>
        <label>
          <p>{{ truncatedDecr }}</p>
          <input v-if="isEditable" type="text" v-model="descrInput" />
        </label>
      </div>
    </div>
    <div class="list__item-icons">
      <button class="list__item-button" @click="handleEditMode">
        <i class="fa-solid fa-pen-to-square"></i>
      </button>
      <button class="list__item-button list__item-button_danger">
        <i class="fa-solid fa-trash" style="color: white;"></i>
      </button>
    </div>
  </div>
</template>

<script>
import '@fortawesome/fontawesome-free/css/all.css';

export default {
  name: 'todo-item',
  data() {
    return {
      title: 'Title',
      descr: 'Descr',
      isDone: false,
      titleInput: '',
      descrInput: '',
      isEditable: false
    }
  },
  methods: {
    handleEditMode() {
      this.isEditable = !this.isEditable
      this.handleUpdate()
    },
    // для читабельности вынес это дело
    isEmpty(value) {
      return value.trim() === '';
    },
    // Если у нас путые инпуты, то не даем переписать значение
    // И не выходим из editMode
    handleUpdate() {
        if (this.isEmpty(this.descrInput) || this.isEmpty(this.titleInput)) {
          alert('Не оставляйте пустые значения!');
        } else {
          this.title = this.titleInput.trim();
          this.descr = this.descrInput.trim();
        }
    }
  },
  // для того, чтобы у инпутов были дефолтные значения
  created() {
    this.titleInput = this.title;
    this.descrInput = this.descr;
  },
  computed: {
    truncatedTitle() {
      return this.title.length > 10 ? this.title.slice(0, 10) + '...' : this.title
    },
    truncatedDecr() {
      return this.descr.length > 10 ? this.descr.slice(0, 10) + '...' : this.descr
    },
  }
}
</script>

<style lang="scss" scoped>
.list {
  &__item {
    display: flex;
    max-width: 600px;
    align-items: center;
    width: 100%;
    justify-content: space-between;
    border: 1px solid #ccc;
    padding: 20px;
    border-radius: 10px;

    &_done {
      background-color: #e7e7e7;

      h3,
      p {
        text-decoration: line-through;
      }
    }

    &-wrapper {
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 10px;
    }

    input[type='checkbox'] {
      width: 20px;
      height: 20px;
      margin: 0;
      accent-color: #2d2d2d;
    }

    input {
      padding: 5px;
      border-radius: 5px;
      border: #2d2d2d 1px solid;
      font-size: 12px;
    }

    &-text {
      text-align: left;

      h3,
      p {
        margin: 0;
      }
    }

    &-icons {
      display: flex;
      gap: 5px;
    }

    &-button {
      padding: 10px;
      width: 45px;
      height: 45px;
      outline: none;
      border: none;
      border-radius: 5px;
      border: 1px solid #ccc;
      cursor: pointer;

      &_danger {
        background-color: rgb(195, 32, 32);
      }
    }
  }
}
</style>