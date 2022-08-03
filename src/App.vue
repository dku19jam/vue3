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
              @click="onSubmit">
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
        {{todo.subject}}
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
    const todos = ref([
      {
        id:1, subject: "휴대폰 사기",
      },
      {
        id:2, subject: "장보기",
      }
    ]);

    const onSubmit = () => {
      if (todo.value === '') {
        hasError.value = true;
      }
      else{
        console.log(todo.value);
        todos.value.push({
          id: Date.now(),
          subject: todo.value,
        });
        hasError.value = false;
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
      onSubmit,
      onToggle,

    };
  }
}
</script>

<style>
.name {
  color: #d06767;
}
</style>
