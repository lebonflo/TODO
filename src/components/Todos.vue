<template>
  <section class="todoapp">
    <header class="header">
      <h1>To-do List</h1>
      <input
        v-model="newTodo"
        @keyup.enter="addTodo"
        type="text"
        class="new-todo"
        placeholder="Ajouter une tâche"
      />
    </header>
    <div class="main">
      <ul class="todo-list">
        <li
          v-for="todo in filteredTodos"
          :key="todo"
          :class="{ completed: todo.completed, editing: todo === editing }"
          class="todo"
        >
          <div class="view">
            <input v-model="todo.completed" type="checkbox" class="toggle" />
            <label @dblclick="editTodo(todo)">{{ todo.name }}</label>
            <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
          </div>
          <input
            type="text"
            class="edit"
            v-model="todo.name"
            @keyup.enter="doneEdit"
            @blur="doneEdit"
            @keyup.esc="cancelEdit"
            v-focus="todo === editing"
          />
        </li>
      </ul>
    </div>
    <footer class="footer" v-show="hasTodos">
      <span class="todo-count"
        ><strong>{{ remaining }}</strong> Tache<span v-if="remaining >= 2"
          >s</span
        >
        à faire</span
      >
      <ul class="filters">
        <li>
          <a
            href=""
            :class="{ selected: filter === 'all' }"
            @click.prevent="filter = 'all'"
            >Toutes</a
          >
        </li>
        <li>
          <a
            href=""
            :class="{ selected: filter === 'todo' }"
            @click.prevent="filter = 'todo'"
            >A faire</a
          >
        </li>
        <li>
          <a
            href=""
            :class="{ selected: filter === 'done' }"
            @click.prevent="filter = 'done'"
            >Faite</a
          >
        </li>
      </ul>
      <button
        class="clear-completed"
        v-show="completed"
        @click="deleteCompleted"
      >
        Supprimer tâches faites
      </button>
    </footer>
  </section>
</template>

<script>
export default {
  data() {
    return {
      todos: [],
      newTodo: "",
      filter: "all",
      editing: null,
      oldTodo: "",
    };
  },
  methods: {
    doneEdit() {
      this.editing = null;
    },
    cancelEdit() {
      this.editing.name = this.oldTodo;
      this.doneEdit();
    },
    editTodo(todo) {
      this.editing = todo;
      this.oldTodo = todo.name;
    },
    addTodo() {
      this.todos.push({
        completed: false,
        name: this.newTodo,
      });
      this.newTodo = "";
    },
    deleteTodo(todo) {
      this.todos = this.todos.filter((i) => i !== todo);
    },
    deleteCompleted() {
      this.todos = this.todos.filter((todo) => !todo.completed);
    },
  },

  computed: {
    remaining() {
      return this.todos.filter((todos) => !todos.completed).length;
    },
    completed() {
      return this.todos.filter((todos) => todos.completed).length;
    },
    filteredTodos() {
      if (this.filter === "todo") {
        return this.todos.filter((todo) => !todo.completed);
      } else if (this.filter === "done") {
        return this.todos.filter((todo) => todo.completed);
      }
      return this.todos;
    },
    hasTodos() {
      return this.todos.length > 0;
    },
  },
  directives: {
    focus(el, value) {
      if (value) {
        el.focus();
      }
    },
  },
};
</script>
<style src="../assets/css/todos.css">
</style>