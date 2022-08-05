<template>
  <router-view></router-view>
  <div class="container">
    <h2>To-DO List</h2>
    <input
        class="form-control"
        type="text"
        v-model="searchText"
        placeholder="search"
        @keyup.enter="searchTodo"
    >
    <hr>
    <TodoSimpleForm @add-todo="addTodo"></TodoSimpleForm>
    <div v-if="!todos.length">There is nothing to display</div>
    <TodoList
        :todos="todos"
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
import {ref, computed, watch,} from "vue";
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
    const searchText = ref("");


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
        const res = await axios.get(`http://localhost:3000/todos?_sort=id&_order=desc&subject_like=${searchText.value}&_page=${page}&_limit=${limit}`);
        numberOfTodos.value = res.headers['x-total-count'];
        todos.value = res.data;
      } catch (err){
        console.log(err);
      }
    }
    getTodos();
    const addTodo = async (todo) => {
      try {
        await axios.post("http://localhost:3000/todos", {
          subject: todo.subject,
          completed: todo.completed,
        });

        getTodos(1);
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

    let timeout = null;

    const searchTodo = () => {
      clearTimeout(timeout);
      getTodos(1);
    };


    watch(searchText, () => {
      clearTimeout(timeout)
      timeout = setTimeout(() => {
        getTodos(1);
      }, 2000);

    });


    const deleteTodo = (index) => {
      const id = todos.value[index].id;
      try {
        axios.delete("http://localhost:3000/todos/" + id);

        getTodos(1);
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
      searchTodo,
      searchText,
      // filteredTodos,
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
