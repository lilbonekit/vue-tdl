<template>
  <div id="app">
    <h1>Todo list</h1>
    <TodoList/>
    <!-- FEATURE BRANCH -->
    <!-- rel noopener -->
    <footer>Made by <a href="https://github.com/lilbonekit/vue-tdl">lilbonekit</a></footer>
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue';

export default {
  name: 'App',
  components: {
    TodoList
  }
}
</script>

<style lang="scss">
* {
  box-sizing: border-box;
}

// вынести в scss
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  max-width: 1200px;
  margin: 0 auto;
}
h1 {
  font-size: 4rem;
}
footer {
  text-align: center;
  font-size: 12px;
  padding: 10px;
  bottom: 20px;
  position: fixed;
  width: 100%;
  max-width: 1200px;
  z-index: -1;
  a {
    color: black;
  }
}
</style>
