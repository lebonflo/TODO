<template>
  <section class="todoapp">
    <header class="header">
      <h1>Todo</h1>
      <input
        v-model="newTodo"
        @keyup.enter="addTodo"
        type="text"
        class="new-todo"
        placeholder="Ajouter une tâche"
      />
    </header>
    <div class="main">
      <input v-model="allDone" type="checkbox" class="toggle-all"/>
      <ul class="todo-list">
        <li
          v-for="todo in filteredTodos"
          :key="todo"
          :class="{ completed: todo.completed }"
          class="todo"
        >
          <div class="view">
            <input v-model="todo.completed" type="checkbox" class="toggle" />
            <label>{{ todo.name }}</label>
          </div>
        </li>
      </ul>
    </div>
    <footer class="footer">
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
    </footer>
  </section>
</template>

<script>
export default {
  data() {
    return {
      allDone: {
        get() {},
        set() {
            console.log()
        },
      },
      todos: [
        {
          name: "Tache de test",
          completed: false,
        },
      ],
      newTodo: "",
      filter: "all",
    };
  },
  methods: {
    addTodo() {
      this.todos.push({
        completed: false,
        name: this.newTodo,
      });
      this.newTodo = "";
    },
  },
  computed: {
    remaining() {
      return this.todos.filter((todos) => !todos.completed).length;
    },
    filteredTodos() {
      if (this.filter === "todo") {
        return this.todos.filter((todo) => !todo.completed);
      } else if (this.filter === "done") {
        return this.todos.filter((todo) => todo.completed);
      }
      return this.todos;
    },
  },
};
</script>
<style src="../assets/css/todos.css">
</style>