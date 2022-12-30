<template>
  <Navbar />

  <main class="container">
    <Alert
      message="Todo title is required"
      :show="showAlert"
      @close="showAlert = false"
      type="danger"
    />

    <section>
      <AddTodoForm @submit="addTodo" />
    </section>

    <section>
      <Todo
        v-for="todo in todos"
        :key="todo.id"
        :title="todo.title"
        :inputTitle="todoTitle"
        @remove="removeTodo(todo)"
        @edit="editTodo"
      />
    </section>
  </main>
</template>

<script>
import Alert from "./components/Alert.vue";
import Navbar from "./components/Navbar.vue";
import AddTodoForm from "./components/AddTodoForm.vue";
import Todo from "./components/Todo.vue";

export default {
  components: {
    Alert,
    Navbar,
    AddTodoForm,
    Todo,
  },
  data() {
    return {
      todoTitle: "",
      todos: [],
      showAlert: false,
    };
  },
  methods: {
    addTodo(title) {
      if (title !== "") {
        this.todos.push({
          title,
          id: this.todos.length + Math.floor(Math.random() * 100),
        });
        //AddTodoForm.ClearTitle();
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

<style scoped></style>
