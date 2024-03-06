<script setup>
/**
 * All imports
 */
import TodoItem from "./components/TodoItem.vue";
import { ref } from "vue";

/**
 * Local state
 */
const todoValue = ref("");
const todos = ref([]);

/**
 * Handler functions
 */
const addTodo = () => {
  // no empty todos
  if (todoValue.value.trim() === "") return

  const createTodo = {
    id: crypto.randomUUID(),
    todo: todoValue.value
  }

  todos.value.push(createTodo)

  // reset imput
  todoValue.value = ""
}

const editTodo = (id, todo) => {
  // console.log({ id, todo });
  const filterTodos = todos.value.filter(i => i.id !== id)

  const newTodo = {
    id,
    todo
  }

  todos.value = [...filterTodos, newTodo]
}

const deleteTodo = (id) => {
  todos.value = todos.value.filter(i => i.id !== id)
}

</script>

<template>
  <article class="container">
    <form @submit.prevent="addTodo">
      <h2>Todo list</h2>
      <label>
        <input type="text" v-model="todoValue" placeholder="add todo">
      </label>
      <button type="submit">Add</button>
    </form>
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        <TodoItem :todoContent="todo.todo" :id="todo.id" @edit="editTodo" @delete="deleteTodo" />
      </li>

      <h2 v-if="todos.length === 0">🪹</h2>
    </ul>
  </article>
</template>

<style scoped>
h2 {
  text-align: center;
  font-size: 25px;
}

article {
  max-width: 500px;
  padding: 10px;
  margin: 40px auto;
}

ul {
  margin-top: 10px;
  padding: 5px 20px;
}
</style>