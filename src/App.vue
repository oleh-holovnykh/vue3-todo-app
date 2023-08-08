<script>
import todos from './data/todos';
import StatusFilter from './components/StatusFilter.vue';
import TodoItem from "@/components/TodoItem.vue";

export default {
  components: {
    TodoItem,
    StatusFilter,
  },
  data() {
    let todos = [];
    const jsonData = localStorage.getItem('todos') || '[]';

    try {
      todos = JSON.parse(jsonData);
    } catch (e) {
    }

    return {
      todos,
      title: '',
      status: 'all',
    }
  },
  computed: {
    activeTodos() {
      return this.todos.filter(todo => !todo.completed)
    }
  },
  watch: {
    todos: {
      deep: true,
      handler() {
        localStorage.setItem('todos', JSON.stringify(this.todos));
      }
    }
  },
  methods: {
    handleSubmit() {
      this.todos.push({
        id: Date.now(),
        title: this.title,
        completed: false,
      });
      this.title = ''
    }
  }
}
</script>
<template>
  <div class="todoapp">
    <h1 class="todoapp__title">todos</h1>

    <div class="todoapp__content">
      <header class="todoapp__header">
        <button
          type="button"
          class="todoapp__toggle-all"
          :class="{ active: activeTodos.length === 0}"
        />

        <form @submit.prevent="handleSubmit">
          <input
            type="text"
            class="todoapp__new-todo"
            placeholder="What needs to be done?"
            v-model="title"
          />
        </form>
      </header>

      <section class="todoapp__main">
        <TodoItem
          v-for="(todo, index) of todos"
          :key="todo.id"
          :todo="todo"
          @update="todos[index] = $event"
          @delete="todos.splice(index, 1)"
        />

      </section>

      <footer class="todoapp__footer">
        <span class="todo-count"> {{ activeTodos.length }} items left </span>

        <StatusFilter
          v-model="status"
        />

        <button
          type="button"
          class="todoapp__clear-completed"
          v-if="activeTodos.length > 0"
        >
          Clear completed
        </button>
      </footer>
    </div>

    <div class="notification is-danger is-light has-text-weight-normal">
      <button type="button" class="delete"/>

      Unable to add a todo
      <br/>
      Unable to delete a todo
      <br/>
      Unable to update a todo
    </div>
  </div>
</template>
<style>
</style>
