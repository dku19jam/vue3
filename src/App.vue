<template>
  <div class="container">
    <h2>To-DO List</h2>
    <input
        class="form-control"
        type="text"
        v-model="searchText"
        placeholder="search"
    >
    <hr>
    <TodoSimpleForm @add-todo="addTodo"></TodoSimpleForm>

    <div v-if="!filteredTodos.length">There is nothing to display</div>
    <TodoList
        :todos="filteredTodos"
        @toggle-todo="toggleTodo"
        @delete-todo="deleteTodo">
    </TodoList>

  </div>
</template>

<script>
import {ref,computed} from "vue";
import TodoSimpleForm from "@/components/TodoSimpleForm";
import TodoList from "@/components/TodoList";
import axios from "axios";


export default {
  components:{
    TodoSimpleForm,
    TodoList,
  },
  setup() {
    const todos = ref([]);
    const todoStyle ={
      textDecoration: 'line-through',
      color: 'gray',
    }

    const addTodo = (todo) => {
      console.log('start');
      axios.post("http://localhost:3000/todos", {
        subject: todo.subject,
        completed: todo.completed,
      }).then(res => {
        console.log(res)
        todos.value.push(res.data);
      }).catch(err =>{
        console.log(err)
      });
      console.log('hello');
    };

    const toggleTodo = (index) => {
      console.log(todos.value[index]);
      todos.value[index].completed = !todos.value[index].completed;
      console.log(todos.value[index]);
    }

    const searchText = ref("");

    const filteredTodos =computed(()=>{
      if (searchText.value) {
        return todos.value.filter( todo => {
          return todo.subject.includes(searchText.value);
        });
      }
      return todos.value;
    })

    const deleteTodo = (index) => {
      todos.value.splice(index, 1);
    }

    return {
      todos,
      todoStyle,
      addTodo,
      deleteTodo,
      toggleTodo,
      searchText,
      filteredTodos,
    };
  }
}
</script>

<style>
  .todo {
    color: gray;
    text-decoration: line-through;
  }
</style>
