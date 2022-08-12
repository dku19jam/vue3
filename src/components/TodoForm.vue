<template>
  <div v-if="loading">
    loading...
  </div>
  <form
    v-else
    @submit.prevent="onSave">
    <div class="row">
      <div class="col-6">
        <div class="form-group">
          <label>Subject</label>
          <input v-model="todo.subject" type="text" class="form-control">
          <div v-if="subjectError" style="color: red">
            {{subjectError}}
          </div>
        </div>
      </div>
      <div v-if="editing" class="col-6">
        <div class="form-group">
          <label>Status</label>
          <div>
            <button class="btn"
                    :class="todo.completed ? 'btn-success' : 'btn-danger'"
                    @click="toggleTodoStatus">
              {{todo.completed ? 'Completed': 'Incomplete'}}
            </button>
          </div>
        </div>
      </div>
      <div class="col-12">
        <div class="form-group">
          <label>Body</label>
          <textarea v-model="todo.body" class="form-control" id="" cols="30" rows="10"></textarea>
        </div>
      </div>
    </div>
    <button
      type="submit"
      class="btn btn-primary"
      :disabled="!todoUpdated"
    >{{ editing ? 'Update' : 'Create'}}</button>
    <button class="btn btn-outline-dark ml-2"
            @click="moveToTodoListPage">Cancel</button>
  </form>
  <Toast
    v-if="showToast"
    :message="toastMessage"
    :type="toastAlertType"
  />
</template>

<script>
import {useRoute, useRouter} from 'vue-router';
import axios from "axios";
import { ref, computed } from "vue";
import _ from 'lodash';
import Toast from "@/components/Toast";
import {useToast} from "@/hooks/toast";

export default {
  components:{
    Toast,
  },
  props: {
    editing: {
      type: Boolean,
      default: false,
    }
  },
  setup(props) {

    const route = useRoute();
    const router = useRouter();
    const todo = ref({
      subject: '',
      completed: false,
      body: '',
    });
    const originalTodo = ref(null);
    const todoId = route.params.id;
    const loading = ref(false);
    const subjectError = ref('');
    const {
      showToast,
      toastMessage,
      toastAlertType,
      triggerToast,} = useToast();


    const getTodo = async () => {
      loading.value = true;
      try {
        const res = await axios.get(`http://localhost:3000/todos/${todoId}`);

        todo.value = {...res.data};
        originalTodo.value = {...res.data};

        loading.value = false;
      } catch (error){
        loading.value = false;
        triggerToast('Something went wrong','danger')
      }

    };

    const todoUpdated = computed(() => {
      return !_.isEqual(todo.value, originalTodo.value);
    });

    const toggleTodoStatus = () => {
      todo.value.completed = !todo.value.completed;
    }

    const moveToTodoListPage= () => {
      router.push({
        name: 'Todos'
      })
    }

    const onSave = async () => {
      subjectError.value = '';
      if (!todo.value.subject) {
        subjectError.value = 'Subject is required';
        return;
      }
      try {
        let res;
        const data ={
          subject: todo.value.subject,
          completed: todo.value.completed,
          body: todo.value.body
        }
        if (props.editing) {
          res = await axios.put(`http://localhost:3000/todos/${todoId}`, data);
        } else {
          res = await axios.post(`http://localhost:3000/todos`,data);
          todo.value.subject = '';
          todo.value.body = '';
        }


        originalTodo.value = {...res.data};
        const message = 'Successfullly ' + (props.editing ? "Updated" : "Created");
        triggerToast(message);
      } catch (error) {
        console.log(error);
        triggerToast('Something went wrong', 'danger');
      }
    };

    if (props.editing) {
      getTodo();
    }

    return{
      todo,
      loading,
      toggleTodoStatus,
      moveToTodoListPage,
      onSave,
      showToast,
      todoUpdated,
      toastMessage,
      toastAlertType,
      subjectError,
    }
  },
}
</script>

<style scoped>

</style>