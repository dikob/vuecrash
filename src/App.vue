<template>
  <div id="app">
    <Header />
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Header from './components/layout/Header';
import Todos from './components/Todos';
import AddTodo from './components/AddTodo';
// use for http request
import axios from 'axios';

export default {
  name: 'App',
  components: {
    Header,
    Todos,
    AddTodo
  },
  data() {
    return {

      todos: []

      /* manual entry
      todos: [
        {
          id: 1,
          title: "Todo One",
          completed: true
        },
        {
          id: 2,
          title: "Todo Two",
          completed: true
        },
        {
          id: 3,
          title: "Todo Three",
          completed: false
        }
      ]
      */
    }
  },
  methods: {
    deleteTodo(id) {
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
      .then (res => {this.todos = this.todos.filter(todo => todo.id !== id); return res})
      .catch(err => console.log(err))

      // this is manual delete without going to the db server
      //this.todos = this.todos.filter(todo => todo.id !== id);
    },
    addTodo(newTodo) {
      const { userId, title, completed } = newTodo;

      // post data
      axios.post('http://localhost/todosapi/public/api/todo', {
        userId,
        title,
        completed
      })
      .then(res => {this.todos = [...this.todos, newTodo]; return res})
      .catch(err => console.log(err));

      // manual entry
      //this.todos = [...this.todos, newTodo];
    }
  },
  created() {
    axios.get('http://localhost/todosapi/public/api/todos')
      .then(res => this.todos = res.data)
      .catch(err => console.log(err));
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
</style>
