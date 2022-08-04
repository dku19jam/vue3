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
    <hr>
    <nav aria-label="Page navigation example">
      <ul class="pagination">
        <li v-if="currentPage !== 1" class="page-item">
          <a style="cursor: pointer" class="page-link" @click="getTodos(currentPage-1)">Previous</a>
        </li>
        <li
            v-for="page in numberOfPages"
            :key="page"
            class="page-item"
            :class="currentPage === page ? 'active' : ''"
        >
          <a class="page-link" @click="getTodos(page)" >{{page}}</a></li>
        <li v-if="numberOfPages !== currentPage" class="page-item">
          <a style="cursor: pointer" class="page-link" @click="getTodos(currentPage+1)">Next</a></li>
      </ul>
    </nav>

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
    const numberOfTodos = ref(0);
    const limit = 5;
    const currentPage = ref(1);

    const numberOfPages = computed(()=>{
      return Math.ceil(numberOfTodos.value / limit);
    })

    const todoStyle ={
      textDecoration: 'line-through',
      color: 'gray',
    }

    const getTodos = async (page = currentPage.value) =>{
      currentPage.value = page;
      try {
        const res = await axios.get(`http://localhost:3000/todos?_page=${page}&_limit=${limit}`);
        numberOfTodos.value = res.headers['x-total-count'];
        todos.value = res.data;
      } catch (err){
        console.log(err);
      }
    }
    getTodos();
    const addTodo = async (todo) => {
      try {
        const res = await axios.post("http://localhost:3000/todos", {
          subject: todo.subject,
          completed: todo.completed,
        });
        todos.value.push(res.data);
      } catch (err) {
        console.log('hello');
      }
    };

    const toggleTodo = (index) => {

      const id = todos.value[index].id;
      try {
          axios.patch("http://localhost:3000/todos/" + id,{
          completed: !todos.value[index].completed
        });
        todos.value[index].completed = !todos.value[index].completed;
      } catch (err){
        console.log(err)
      }
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
      const id = todos.value[index].id;
      try {
        const res = axios.delete("http://localhost:3000/todos/" + id);
        console.log(res);
      } catch (err){
        console.log(err)
      }

      todos.value.splice(index, 1);
    }

    return {
      todos,
      todoStyle,
      numberOfPages,
      addTodo,
      deleteTodo,
      toggleTodo,
      getTodos,
      searchText,
      filteredTodos,
      currentPage
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
