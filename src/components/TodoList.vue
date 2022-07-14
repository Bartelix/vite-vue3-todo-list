<script setup lang="ts">
import { computed, defineProps, defineEmits } from 'vue';

enum CATEGORIES {
  Business,
  Personal
}

type todo = {
  content: string,
  category: CATEGORIES,
  done: boolean,
  createdAt: number
}

const props = defineProps<{
	todos: todo[]
}>();

const todos_asc = computed(() =>
  props.todos.sort((a: todo, b: todo) => b.createdAt - a.createdAt)
);

const emit = defineEmits<{
	(e: 'removeTodo', todo: todo): void
}>();

const removeTodo = (todo: todo) => {
  emit('removeTodo', todo)
};
</script>

<template>
	<section class="todo-list">
		<h3>TODO LIST</h3>

		<div
			v-for="todo in todos_asc"
			:class="`todo-item ${todo.done && 'done'}`"
		>
			<label>
				<input type="checkbox" v-model="todo.done" />
				<span :class="`bubble ${todo.category}`"></span>
			</label>
			<div class="todo-content">
				<input type="text" v-model="todo.content" />
			</div>
			<div class="actions">
				<button class="delete" @click="removeTodo(todo)">Delete</button>
			</div>
		</div>
	</section>
</template>
