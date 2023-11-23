<script setup>
import { computed, ref } from "vue";

let id = 0;

const inputValue = ref("");
const todos = ref([]);
const hideCompleted = ref(false);

const addTodo = () => {
  todos.value.push({ id: id++, value: inputValue.value, done: false });
  inputValue.value = "";
};

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo);
};

const filterdTodos = computed(() => {
  return hideCompleted.value ? todos.value.filter((t) => !t.done) : todos.value;
});
</script>

<template>
  <div class="container">
    <h1>Vue TodoList</h1>
    <form @submit.prevent="addTodo">
      <div class="input-button">
        <input v-model="inputValue" @keydown.enter="addTodo" />
        <button>추가</button>
      </div>
      <div class="list-container">
        <li v-for="todo in filterdTodos" :key="todo.id">
          <input type="checkbox" v-model="todo.done" />
          <span :class="{ done: todo.done }">{{ todo.value }}</span>
          <button @click="removeTodo(todo)">X</button>
        </li>
      </div>
    </form>
    <button @click="hideCompleted = !hideCompleted">
      {{ hideCompleted ? "Show All!" : "Hide Completed!" }}
    </button>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.done {
  text-decoration: line-through;
}

.input-button {
  display: flex;
}
</style>
