<script lang="ts">
  import { defineComponent, ref, computed } from 'vue';

  interface Todo {
    id: string;
    label: string;
    completed: boolean;
  }

  const randomId = (): string => {
    return `_${Math.random().toString(35).slice(2, 11)}`;
  };

  export default defineComponent({
    setup() {
      const newTodo = ref("");

      const todos = ref<Todo[]>([])

      const addTodo = (e: KeyboardEvent) => {
        if (e.key === "Enter" && newTodo.value) {
          todos.value.push({ id: randomId(), label: newTodo.value, completed: false})
          newTodo.value = "";
        }
      }

      const completedTodos = computed(() => todos.value.filter(todo => todo.completed === true))
      const activeTodos = computed(() => todos.value.filter(todo => todo.completed === false))

      const removeTodo = (id: string) => {
        todos.value = todos.value.filter(todo => todo.id !== id)
      }

      const completeTodo = (id: string) => {
        todos.value = todos.value.map(todo => {
          if (todo.id === id) {
            return {...todo, completed: true}
          } else {
            return todo
          }
        })
      }

      return {
        newTodo,
        todos,
        addTodo,
        activeTodos,
        completedTodos,
        completeTodo,
        removeTodo
      }
    }
  });
</script>

<template>
  <div class="container">
    <h1 class="header">Todo App</h1>
    <input 
      placeholder="New todo"
      class="input"
      v-model="newTodo"
      @keyup="addTodo"
      />
    <h4>Active</h4>
    <ul class="list">
      <li class="item" v-for="todo in activeTodos" :key="todo.id">
        <span>{{ todo.label }}</span>
        <div class="item-buttons">
          <button class="remove-button" @click="removeTodo(todo.id)">Remove</button>
          <button class="done-button" @click="completeTodo(todo.id)">Done</button>
        </div>
      </li>

    </ul>
    <div v-if="completedTodos.length > 0">
      <h4>Done items</h4>
      <ul>
        <li clas="item" v-for="todo in completedTodos" :key="todo.id">
          <span>{{ todo.label }}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
</style>
