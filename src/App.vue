<script setup lang="ts">
import { ref, onMounted, computed, watch } from 'vue';
import TodoList from './components/TodoList.vue';
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

const todos = ref<todo[]>([]);
const name = ref('');

const input_content = ref('');
const input_category = ref(null);



const addTodo = () => {
  if (input_content.value.trim() === '' || input_category.value == null) return;

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime(),
  });

  input_content.value = '';
  input_category.value = null;
};

const removeTodo = (todo: todo) => {
  console.log(todo)
  todos.value = todos.value.filter((t) => t !== todo);
};

watch(name, (newVal) => {
  localStorage.setItem('name', newVal);
});

watch(
  todos,
  (newVal) => {
    localStorage.setItem('todos', JSON.stringify(newVal));
  },
  { deep: true }
);

onMounted(() => {
  name.value = localStorage.getItem('name') || '';
  todos.value = JSON.parse(localStorage.getItem('todos') as string) || [];
});
</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        What's up, <input type="text" placeholder="Name here" v-model="name" />
      </h2>
    </section>
    <section class="create-todo">
      <h3>CREATE A TODO</h3>

      <form @submit.prevent="addTodo">
        <h4>What's on your todo list?</h4>
        <input
          type="text"
          placeholder="e.g. make a video"
          v-model="input_content"
        />
        <h4>Pick a category</h4>
        <div class="options">
          <label>
            <input
              type="radio"
              name="category"
              value="business"
              v-model="input_category"
            />
            <span class="bubble business"></span>
            <div>Business</div>
          </label>
          <label>
            <input
              type="radio"
              name="category"
              value="personal"
              v-model="input_category"
            />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>

        <input type="submit" value="Add todo" />
      </form>
    </section>
    <TodoList
      :todos="todos"
      @removeTodo="removeTodo"
    />
  </main>
</template>

<style scoped></style>
