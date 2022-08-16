<template>
	<div>
		<div class="d-flex justify-content-between mb-3">
			<h2>To-DO List</h2>
			<button class="btn btn-primary" @click="moveToCreatePage">
				Create Todo
			</button>
		</div>
		<input
			class="form-control"
			type="text"
			v-model="searchText"
			placeholder="search"
			@keyup.enter="searchTodo"
		/>
		<hr />
		<div v-if="!todos.length">There is nothing to display</div>
		<TodoList
			:todos="todos"
			@toggle-todo="toggleTodo"
			@delete-todo="deleteTodo"
		>
		</TodoList>
		<hr />
		<Pagination
			v-if="todos.length"
			:numberOfPages="numberOfPages"
			:currentPage="currentPage"
			@click="getTodos"
		/>
	</div>
</template>

<script>
import { ref, computed, watch } from 'vue';
import TodoList from '@/components/TodoList';
import axios from '@/axios';
import { useToast } from '@/hooks/toast';
import { useRouter } from 'vue-router';
import Pagination from '@/components/pagination.vue';

export default {
	components: {
		TodoList,
		Pagination,
	},
	setup() {
		const router = useRouter();
		const todos = ref([]);
		const numberOfTodos = ref(0);
		const limit = 5;
		const currentPage = ref(1);
		const searchText = ref('');
		const numberOfPages = computed(() => {
			return Math.ceil(numberOfTodos.value / limit);
		});

		const { showToast, toastMessage, toastAlertType, triggerToast } =
			useToast();

		const todoStyle = {
			textDecoration: 'line-through',
			color: 'gray',
		};

		const moveToCreatePage = () => {
			router.push({
				name: 'TodoCreate',
			});
		};

		const getTodos = async (page = currentPage.value) => {
			currentPage.value = page;
			try {
				const res = await axios.get(
					`todos?_sort=id&_order=desc&subject_like=${searchText.value}&_page=${page}&_limit=${limit}`
				);
				numberOfTodos.value = res.headers['x-total-count'];
				todos.value = res.data;
			} catch (err) {
				console.log(err);
				triggerToast('Something went wrong', 'danger');
			}
		};
		getTodos();
		const addTodo = async (todo) => {
			try {
				await axios.post('todos', {
					subject: todo.subject,
					completed: todo.completed,
				});

				getTodos(1);
			} catch (err) {
				triggerToast('Something went wrong', 'danger');
			}
		};

		const toggleTodo = async (index, checked) => {
			const id = todos.value[index].id;
			try {
				await axios.patch('todos/' + id, {
					completed: checked,
				});
				todos.value[index].completed = checked;
			} catch (err) {
				console.log(err);
				triggerToast('Something went wrong', 'danger');
			}
		};

		let timeout = null;

		const searchTodo = () => {
			clearTimeout(timeout);
			getTodos(1);
		};

		watch(searchText, () => {
			clearTimeout(timeout);
			timeout = setTimeout(() => {
				getTodos(1);
			}, 2000);
		});

		const deleteTodo = async (id) => {
			try {
				await axios.delete('todos/' + id);

				getTodos(1);
			} catch (err) {
				console.log(err);
				triggerToast('Something went wrong', 'danger');
			}
		};

		return {
			todos,
			todoStyle,
			numberOfPages,
			addTodo,
			deleteTodo,
			toggleTodo,
			getTodos,
			searchTodo,
			searchText,
			// filteredTodos,
			currentPage,
			toastMessage,
			toastAlertType,
			showToast,
			moveToCreatePage,
		};
	},
};
</script>

<style>
.todo {
	color: gray;
	text-decoration: line-through;
}
</style>
