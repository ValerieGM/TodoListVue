<template>
  <div id="app">
    <AddToDo v-on:add-todo="addTodo"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteToDo"/>
  </div>
</template>

<script>

import Todos from '../components/Todos';
import AddToDo from '../components/AddTodo';
import axios from 'axios';

export default {
  name: 'Home',
  components:{
    Todos,
    AddToDo
  },
  data() {
    return{
      todos: []
    }
  },
  methods: {
    deleteToDo(id) {
      axios.delete(`http://jsonplaceholder.typicode.com/todos/${id}`)
      .then(() => this.todos = this.todos.filter(todo => todo.id != id))
      .catch(error => console.log(error));

      // this.todos = this.todos.filter(todo => todo.id != id);
    },
    addTodo(newTodo) {

      // Destructuring to pull out from newTodo
      const { title, completed } =  newTodo;

      axios.post('http://jsonplaceholder.typicode.com/todos',
      {
        title,
        completed
      })
      .then(response => this.todos = [...this.todos, response.data])
      .catch(error => console.log(error));
      // this.todos = [...this.todos, newTodo];
    }
  },
  created() {
    // NB:: Prosmises
    // HTTP library
    axios.get('http://jsonplaceholder.typicode.com/todos?_limit=11')
    .then((response) => this.todos = response.data)
    .catch((error) => console.log(error));
    }
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
}

.btn {
  display: inline-block;
  border: none;
  background: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}

.btn-hover {
  background: #666;
}
</style>

