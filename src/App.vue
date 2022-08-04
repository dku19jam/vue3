<template>
  <div class="container">
    <h2>To-DO List</h2>
    <TodoSimpleForm @add-todo="addTodo"></TodoSimpleForm>

    <div v-if="!todos.length">추가된 Todo가 없습니다</div>
    <TodoList
        :todos="todos"
        @toggle-todo="toggleTodo"
        @delete-todo="deleteTodo">
    </TodoList>

  </div>
</template>

<script>
import {ref} from "vue";
import TodoSimpleForm from "@/components/TodoSimpleForm";
import TodoList from "@/components/TodoList";


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
      todos.value.push(todo);
    };

    const toggleTodo = (index) => {
      console.log(todos.value[index]);
      todos.value[index].completed = !todos.value[index].completed;
      console.log(todos.value[index]);
    }

    const deleteTodo = (index) => {
      todos.value.splice(index, 1);
    }

    return {
      todos,
      todoStyle,
      addTodo,
      deleteTodo,
      toggleTodo,
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
