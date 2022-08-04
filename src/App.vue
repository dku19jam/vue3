<template>
  <div class="container">
    <h2>To-DO List</h2>
    <form @submit.prevent="onSubmit">
      <div class="d-flex">
        <div class="flex-grow-1">
          <input
            class="form-control"
            type="text"
            v-model="todo">
      </div>
        <div>
          <button
              class="btn btn-primary"
              type="submit">
            Add
          </button>
        </div>
      </div>
    </form>
    <div v-show="hasError" style="color: red">This field cannot be empty</div>
    <div v-if="!todos.length">추가된 Todo가 없습니다</div>
    <div
        v-for="(todo, index) in todos"
        :key="todo.id"
        class="card mt-2">
      <div class="card-body p-2 d-flex align-items-center">
        <div class="form-check flex-grow-1" >
          <input
              class="form-check-input"
              type="checkbox"
              v-model="todo.completed"
          >
          <label
              class="form-check-label"
              :class="{todo: todo.completed}">
            {{todo.subject}}
          </label>
        </div>
        <div>
          <button
              class="btn btn-danger btn-sm"
              @click="deleteTodo(index)"
          >Delete</button>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import {ref} from "vue";

export default {
  setup() {
    const toggle = ref(false);
    const todo = ref('최재민');
    const hasError = ref(false);
    const todos = ref([]);
    const todoStyle ={
      textDecoration: 'line-through',
      color: 'gray',
    }

    const onSubmit = () => {
      if (todo.value === '') {
        hasError.value = true;
      }
      else{
        console.log(todo.value);
        todos.value.push({
          id: Date.now(),
          subject: todo.value,
          completed: true,
        });
        hasError.value = false;
        todo.value = '';
      }
    };

    const onToggle = () => {
      toggle.value = !toggle.value;
    };

    const deleteTodo = (index) => {
      todos.value.splice(index, 1);
    }

    return {
      hasError,
      toggle,
      todo,
      todos,
      todoStyle,
      onSubmit,
      onToggle,
      deleteTodo,

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
