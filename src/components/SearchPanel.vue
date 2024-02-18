<template>
    <div class="search">
      <h2>Search:</h2>
      <input 
        type="text"
        placeholder="Search..."
        v-model="query"
      >
      <div class="search__filters">
        <button 
          class="search__filters-button"
          :class="{ 'search__filters-button_active': activeFilter === true }"
          @click="handleFilter('completed')"
        >
          Completed
        </button>
        <button 
          class="search__filters-button"
          :class="{ 'search__filters-button_active': activeFilter === false }"
          @click="handleFilter('pending')"
        >
          Pending
        </button>
        <button 
          class="search__filters-button search__filters-button_text"
          @click="handleFilter()"
        >
          Reset filters
        </button>
      </div>
    </div>
</template>

<script>
export default {
  name: 'search-panel',
  data() {
    return {
      query: '',
      activeFilter: null
    }
  },
  methods: {
    handleFilter(type) {
      switch(type) {
        case 'completed':
          this.activeFilter = true
          break
        case 'pending':
          this.activeFilter = false
          break
        default:
          this.activeFilter = null
      }
    }
  },
  watch: {
    activeFilter(newFilter) {
      this.$emit('filter-to-parent', newFilter)
    },
    query(newQuery) {
      this.$emit('query-to-parent', newQuery)
    }
  }
}
</script>

<style lang="scss" scoped>
.search {
  display: flex;
  flex-direction: column;
  background-color: #2d2d2d;
  padding: 10px;
  flex-grow: 1;
  border-radius: 20px;
  height: fit-content;
  gap: 10px;
  &__filters {
    display: flex;
    gap: 5px;
    &-button {
      padding: 5px 10px;
      border-radius: 10px;
      border: none;
      cursor: pointer;
      &_active {
        background-color: #ccc;
      }
      &_text {
        background-color: transparent;
        color: white;
        font-weight: 700;
      }

    }
  }

  h2 {
    color: white;
  }

  input {
    padding: 15px;
    border-radius: 10px;
    outline: none;
    border: none;
  }
}
</style>