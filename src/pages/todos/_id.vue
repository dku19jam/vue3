<template>
  <h1>Todo Page</h1>
  <TodoForm :editing="true"></TodoForm>
</template>

<script>
import TodoForm from "@/components/TodoForm";

export default {
  components: {
    TodoForm,
  }
}
</script>

<style >

</style>