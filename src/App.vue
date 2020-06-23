<template>
  <div id="app" class="container">
    <img src="./assets/logo.png" class="logo" />
    <h1 class="heading">Vue todo app</h1>
    <todo-list></todo-list>
  </div>
</template>

<script>
import TodoList from './components/TodoList';

export default {
  name: 'App',
  components: {
    TodoList
  }
};
</script>
