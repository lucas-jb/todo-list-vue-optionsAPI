<template>
  <Navbar />

  <main class="container">
    <Modal
      :show="editTodoForm.show"
      @close="editTodoForm.show = false"
      @submit="updateTodo"
    >
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
        <div class="footer">
          <div class="infoModalFooter">
            <div>Esc to exit - Enter to submit</div>
          </div>
          <div class="editTodoModalFooter">
            <Btn class="editTodoSubmitBtn" @click="updateTodo">Submit</Btn>
            <Btn variant="danger" @click="editTodoForm.show = false">Close</Btn>
          </div>
        </div>
      </template>
    </Modal>
    <Alert
      :message="alert.message"
      :show="alert.show"
      @close="alert.show = false"
      :type="alert.type"
    />

    <section>
      <AddTodoForm :isLoading="buttonLoading" @submit="addTodo" />
    </section>

    <section>
      <Spinner class="todo-loading" v-if="isLoading" />
      
      <div v-else>
        <Todo
          v-for="todo in todos"
          :key="todo.id"
          :title="todo.title"
          :inputTitle="todoTitle"
          @remove="removeTodo(todo)"
          @edit="showEditTodoForm(todo)"
        />
      </div>
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
import Spinner from "./components/Spinner.vue";
import axios from "axios";


export default {
  components: {
    Alert,
    Navbar,
    AddTodoForm,
    Todo,
    Modal,
    Btn,
    Spinner,
  },
  data() {
    return {
      todoTitle: "",
      todos: [],
      alert: {
        show: false,
        message: "",
        type: "",
      },
      showEditTodoModal: false,
      editTodoForm: {
        show: false,
        todo: {
          id: 0,
          title: "",
        },
      },
      isLoading: false,
      buttonLoading: false,
    };
  },
  created() {
    this.fetchTodo();
  },
  methods: {
    async fetchTodo() {
      this.isLoading = true;
      try {
        const res = await axios.get("api/todos");
        this.todos = await res.data;
      } catch (e) {
        this.showAlert("There was a problem loading todos");
      }
      this.isLoading = false;
    },
    showAlert(message, type = "danger") {
      this.alert.message = message;
      this.alert.show = true;
      this.alert.type = type;
    },
    async addTodo(title) {
      this.buttonLoading = true;
      if (title !== "") {
        const res = await axios.post("api/todos", { title });
        this.todos.push(res.data);
      } else {
        this.showAlert("Todo title is required");
      }
      this.buttonLoading = false;
    },
    async removeTodo(todo) {
      await axios.delete(`api/todos/${todo.id}`);
      this.todos = this.todos.filter((t) => t.id !== todo.id);
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
.footer {
  display: flex;
  justify-content: space-between;
}
.editTodoSubmitBtn {
  margin-right: 10px;
}
.infoModalFooter {
  display: flex;
  justify-content: start;
  padding: 30px;
  color: var(--text-color);
}
.todo-loading {
  margin-top: 30px;
}
</style>
