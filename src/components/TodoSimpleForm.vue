<template>
  <form @submit.prevent="onSubmit">
    <div class="d-flex">
      <div class="flex-grow-1">
        <input
            class="form-control"
            type="text"
            v-model="todo"
            placeholder="Type new to-do"
        >
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
</template>

<script>
import {ref} from 'vue';

export default {
  emits: ["add-todo"],
  setup(props, context){
    const todo = ref('');
    const hasError = ref(false);
    const onSubmit = () => {
      if (todo.value === '') {
        hasError.value = true;
      }
      else{
        context.emit('add-todo',{
          id: Date.now(),
          subject: todo.value,
          completed: false,
        });
        hasError.value = false;
        todo.value = '';
      }
    };

    return{
      todo,
      hasError,
      onSubmit,
    }

  }
}
</script>
<style></style>


