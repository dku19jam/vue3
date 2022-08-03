<template>
  <div :class="nameClass">{{ name }}</div>
  <input :type="type" :value="name">
  <button
      class="btn btn-primary"
      @:click="updateName">
    Click
  </button>
</template>

<script>
import {ref} from "vue";

export default {
  setup() {
    const name = ref('최재민');
    const type = ref('text');
    const nameClass = ref('name')

    // const greeting = (name) => {
    //   return 'Hello, ' + name;
    // };
    //
    // const greet = greeting(name);

    const updateName = () => {
      name.value = '최재민1';
      type.value = 'number';
    };

    return {
      name,
      updateName,
      type,
      nameClass,

    };
  }
}
</script>

<style>
.name {
  color: #d06767;
}
</style>
