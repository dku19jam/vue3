<template>
  <div class="container">
    <h2>To-DO List</h2>
    <form
        @submit.prevent="onSubmit"
        class="d-flex">
      <div class="flex-grow-1 mr-2">
        <input
            class="form-control"
            type="text"
            v-model="todo">
      </div>
      <div>
        <button
            class="btn btn-primary"
            @click="onSubmit">
          Add
        </button>
      </div>
    </form>
    {{todos}}
  </div>
</template>

<script>
import {ref} from "vue";

export default {
  setup() {
    const todo = ref('최재민');
    const todos = ref([]);


    const onSubmit = () => {
      console.log(todo.value)
      todos.value.push({
        id: Date.now(),
        subject: todo.value,
      })
    };

    return {
      todo,
      todos,
      onSubmit,
    };
  }
}
</script>

<style>
.name {
  color: #d06767;
}
</style>
