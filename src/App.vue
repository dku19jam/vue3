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
    <div
        v-for="todo in todos"
        :key="todo.id"
        class="card mt-2">
      <div class="card-body p-2">
        <div class="form-check">
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

    return {
      hasError,
      toggle,
      todo,
      todos,
      todoStyle,
      onSubmit,
      onToggle,

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
