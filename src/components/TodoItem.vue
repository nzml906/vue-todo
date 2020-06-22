<template>
  <div class="todo-item">
    <div class="todo-item-left">
      <input type="checkbox" v-model="completed" @change="doneEdit" />
      <div
        v-if="!editing"
        @dblclick="editTodo"
        class="todo-item-label"
        :class="{ completed: completed }"
      >
        {{ title }}
      </div>
      <input
        v-else
        class="todo-item-edit"
        type="text"
        v-model="title"
        @blur="doneEdit"
        @keyup.enter="doneEdit"
        @keyup.esc="cancelEdit"
        v-focus
      />
    </div>
    <div>
      <button @click="pluralize">Plural</button>
      <span class="remove-item" @click="removeTodo(todo.id)">&times;</span>
    </div>
  </div>
</template>

<script>
import { EventBus } from '../event-bus';

export default {
  name: 'todo-item',
  props: {
    todo: {
      type: Object,
      required: true
    },
    checkAll: {
      type: Boolean,
      required: true
    }
  },
  data() {
    return {
      id: this.todo.id,
      title: this.todo.title,
      completed: this.todo.completed,
      editing: this.todo.editing,
      beforeEditCache: ''
    };
  },
  created() {
    EventBus.$on('pluralize', this.handlePluralize);
  },
  beforeDestroy() {
    EventBus.$off('pluralize', this.handlePluralize);
  },
  watch: {
    checkAll() {
      this.completed = this.checkAll ? true : this.todo.completed;
    }
  },
  directives: {
    focus: {
      inserted: function(el) {
        el.focus();
      }
    }
  },
  methods: {
    removeTodo(id) {
      this.$store.dispatch('deleteTodo', id);
    },
    editTodo() {
      this.beforeEditCache = this.title;
      this.editing = true;
    },
    doneEdit() {
      if (this.title.trim() == '') {
        this.title = this.beforeEditCache;
      }
      this.editing = false;
      this.$store.dispatch('updateTodo', {
        id: this.id,
        title: this.title,
        completed: this.completed,
        editing: this.editing
      });
    },
    cancelEdit() {
      this.title = this.beforeEditCache;
      this.editing = false;
    },
    pluralize() {
      EventBus.$emit('pluralize');
    },
    handlePluralize() {
      this.title = this.title + 's';
      this.$store.dispatch('updateTodo', {
        id: this.id,
        title: this.title,
        completed: this.completed,
        editing: this.editing
      });
    }
  }
};
</script>
