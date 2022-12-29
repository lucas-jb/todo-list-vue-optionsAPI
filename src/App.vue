<template>
  <nav class="navbar">
    <img src="./assets/mono.png" width="50" />
    <div class="brand">Todo list App</div>
  </nav>

  <main class="container">
    <Alert
      message="Todo title is required"
      :show="showAlert"
      @close="showAlert = false"
      type="danger"
    ></Alert>
    <section>
      <form class="add-todo-form">
        <input v-model="todoTitle" type="text" placeholder="Todo title" />
        <div>
          <button @click.prevent="addTodo">Add Todo</button>
        </div>
      </form>
    </section>

    <section>
      <div v-for="todo in todos" class="todo" :key="todo.id">
        <p>{{ todo.title }}</p>
        <div>
          <button @click="editTodo(todo)" class="edit-todo-btn">e</button>
          <button @click="removeTodo(todo)" class="remove-todo-btn">x</button>
        </div>
      </div>
    </section>
  </main>
</template>

<script>
import Alert from "./components/Alert.vue";

export default {
  components: {
    Alert,
  },
  data() {
    return {
      todoTitle: "",
      todos: [],
      showAlert: false,
    };
  },
  methods: {
    addTodo() {
      if (this.todoTitle !== "") {
        this.todos.push({
          title: this.todoTitle,
          id: this.todos.length + Math.floor(Math.random() * 100),
        });
        this.todoTitle = "";
      } else {
        this.showAlert = true;
      }
    },

    removeTodo(todo) {
      this.todos = this.todos.filter((todoTitle) => todoTitle !== todo);
      this.todoTitle = "";
    },

    editTodo(todo) {
      todo.title = this.todoTitle;
      this.todoTitle = "";
    },
  },
};
</script>

<style scoped>
.navbar {
  display: flex;
  align-items: center;
  background: var(--navbar-color);
  padding: 20px;
  margin-bottom: 30px;
}
.brand {
  font-size: 2rem;
}

.add-todo-form {
  display: flex;
  justify-content: space-between;
}
.add-todo-form input {
  width: 80%;
  border: solid 2px var(--accent-color);
}
.add-todo-form button {
  background: var(--accent-color);
  color: var(--tex-color);
  border: none;
  height: 50px;
}
.todo {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: var(--accent-color);
  margin-top: 30px;
  padding: 0 20px 0 20px;
  border-radius: 20px;
}
.remove-todo-btn {
  border-radius: 50%;
  border: none;
  height: 40px;
  width: 40px;
  font-size: 30px;
  color: var(--tex-color);
  background: var(--danger-color);
  cursor: pointer;
}

.edit-todo-btn {
  border-radius: 50%;
  border: none;
  height: 40px;
  width: 40px;
  font-size: 30px;
  color: var(--tex-color);
  background: var(--edit-color);
  cursor: pointer;
}
</style>
