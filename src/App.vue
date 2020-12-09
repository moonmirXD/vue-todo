<template>
  <div class="container">
    <form @submit.prevent="addNewTodo">
      <div class="form-group">
        <label for="exampleInputEmail1">Todo</label>
        <input type="input" class="form-control" v-model="newTodo" />
      </div>
      <button class="btn btn-primary">Add Todo</button>
    </form>
    <div class="d-block mt-2">
      <button class="btn btn-primary" @click="markAllDone()">
        Mark All Done
      </button>
      <button class="btn btn-primary ml-2" @click="removeAllTodos()">
        Remove All Todos
      </button>
    </div>
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        <h2 :class="{ done: todo.done }" @click="toggleDone(todo)" class="todo">
          {{ todo.content }}
        </h2>
        <button class="btn btn-info" @click="removeTodo(todo)">Remove</button>
      </li>
    </ul>

    <div>
      <ul>
        <li v-for="todo in listTodos" :key="todo.id">{{ todo.title }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import axios from "axios";
export default {
  setup() {
    const API_URL = "http://localhost:1337/";
    const newTodo = ref("");
    const todos = ref([]);
    const listTodos = ref([]);

    function addNewTodo() {
      todos.value.push({
        id: Date.now(),
        done: false,
        content: newTodo.value,
      });
      newTodo.value = "";
    }

    function toggleDone(todo) {
      todo.done = !todo.done;
    }

    function removeTodo(todo) {
      todos.value.splice(todo, 1);
    }

    function markAllDone() {
      todos.value.forEach((element) => {
        element.done = true;
      });
    }

    function removeAllTodos() {
      todos.value = [];
    }

    async function getTodos() {
      return axios.get(`${API_URL}api/todos`).then((response) => {
        console.log(response);
        listTodos.value = response.data;
      });
    }

    async function createTodos() {}

    getTodos();
    return {
      toggleDone,
      todos,
      newTodo,
      addNewTodo,
      removeTodo,
      markAllDone,
      removeAllTodos,
      getTodos,
      listTodos,
      createTodos,
    };
  },
};
</script>

<style>
@import "~bootstrap/dist/css/bootstrap.css";
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: "Arial", Courier, monospace;
  line-height: 1.4;
}

.done {
  text-decoration: line-through;
}

.todo {
  cursor: pointer;
}
</style>
