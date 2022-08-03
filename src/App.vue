<template>
  <input
    type="text"
    v-model="name">

  <button
      class="btn btn-primary"
      @click="onSubmit">
    Click
  </button>
</template>

<script>
import {ref} from "vue";

export default {
  setup() {
    const name = ref('최재민');


    // const greeting = (name) => {
    //   return 'Hello, ' + name;
    // };
    //
    // const greet = greeting(name);

    const onSubmit = () => {
      console.log(name.value)
    };

    return {
      name,
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
