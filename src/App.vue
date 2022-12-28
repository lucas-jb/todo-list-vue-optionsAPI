<script setup>

</script>

<template>
  <nav class="navbar">
    <img src="./assets/logo.svg" width="50"/>
    <div class="brand">Todo list App</div>
  </nav>
  <main class="container">
    <section>
      <form class="add-todo-form">
        <input type="text" placeholder="Todo title"/>
        <div>
          <button>Add Todo</button>
        </div>
      </form>
    </section>

    <section>
      <div class="todo">
        <p>Pasear al perro</p>
        <div>
          <button class="remove-todo-btn">x</button>
        </div>
      </div>
    </section>
  </main>
</template>

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
.todo{
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: var(--accent-color);
  margin-top: 30px;
  padding: 0 20px 0 20px;
  border-radius: 20px;
}
.remove-todo-btn{
  border-radius: 50%;
  border: none;
  height: 40px;
  width: 40px;
  font-size: 30px;
  color: var(--tex-color);
  background: var(--danger-color);
}
</style>
