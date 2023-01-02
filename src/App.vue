<template>
  <Navbar />

  <main class="container">
    <Modal :show="editTodoForm.show">
      <template #header>
        <h2>Edit Todo</h2>
      </template>

      <template #content>
        <form class="editTodoForm">
          <div><label>Todo Title</label></div>
          <input type="text" v-model="editTodoForm.todo.title" />
        </form>
      </template>

      <template #footer>
        <div class="editTodoModalFooter">
          <Btn class="editTodoSubmitBtn" @click="updateTodo">Submit</Btn>
          <Btn type="danger" @click="editTodoForm.show = false">Close</Btn>
        </div>
      </template>
    </Modal>
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
        @edit="showEditTodoForm(todo)"
      />
    </section>
  </main>
</template>

<script>
import Alert from "./components/Alert.vue";
import Navbar from "./components/Navbar.vue";
import AddTodoForm from "./components/AddTodoForm.vue";
import Todo from "./components/Todo.vue";
import Modal from "./components/Modal.vue";
import Btn from "./components/Btn.vue";

export default {
  components: {
    Alert,
    Navbar,
    AddTodoForm,
    Todo,
    Modal,
    Btn,
  },
  data() {
    return {
      todoTitle: "",
      todos: [],
      showAlert: false,
      showEditTodoModal: false,
      editTodoForm: {
        show: false,
        todo: {
          id: 0,
          title: "",
        },
      },
    };
  },
  methods: {
    addTodo(title) {
      if (title !== "") {
        this.todos.push({
          title,
          id: this.todos.length + Math.floor(Math.random() * 100),
        });
      } else {
        this.showAlert = true;
      }
    },
    removeTodo(todo) {
      this.todos = this.todos.filter((todoTitle) => todoTitle !== todo);
    },

    showEditTodoForm(todo) {
      this.editTodoForm.show = true;
      this.editTodoForm.todo = { ...todo };
    },

    updateTodo() {
      const todo = this.todos.find(
        (todo) => todo.id === this.editTodoForm.todo.id
      );
      todo.title = this.editTodoForm.todo.title;
      this.editTodoForm.show = false;
    },
  },
};
</script>

<style scoped>
.editTodoForm > input {
  width: 100%;
  height: 30px;
  border: 1px solid var(--accent-color);
}
.editTodoModalFooter {
  display: flex;
  justify-content: end;
  padding: 10px;
}
.editTodoSubmitBtn {
  margin-right: 10px;
}
</style>
