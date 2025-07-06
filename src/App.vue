<!-- 
  TODO: Рекомендуется разбить на компоненты:
  - TodoForm для формы добавления
  - TodoList для списка задач
  - TodoItem для отдельной задачи
-->
<template>
  <div class="todo-app">
    <!-- TODO: семантический тег <header> -->
    <h1>Todo app</h1>
    <h3>Lucas Viga</h3>
    
    <div class="container">
    <!-- TODO: Вынести в компонент TodoForm.vue -->
    <div class="add-todo">
      <!-- 
        TODO: Неправильный порядок атрибутов. Должен быть:
        1. v-директивы (v-model)
        2. class, placeholder и другие основные атрибуты
        3. События (@click, @keyup)
      -->
      <input 
        v-model="newTodoText" 
        @keyup.enter="addTodo"
        placeholder="Add a new task"
        class="todo-input"
      >
      <!-- TODO: Добавить aria-label для доступности -->
      <button @click="addTodo" class="add-button">+</button>
    </div>

    <!-- TODO: Вынести в компонент TodoList.vue -->
    <div class="todo-lists">
      <div class="todo-column">
        <h2>Task to do -{{ newTodos.length }}</h2>
        <TransitionGroup name="todo" tag="ul">
          <li 
            v-for="todo in newTodos" 
            :key="todo.id"
            class="todo-item"
          >
            <span>{{ todo.text }}</span>
            <div class="todo-actions">
              <!-- TODO: Добавить aria-label для доступности -->
              <button @click="completeTodo(todo)" class="action-button">✓</button>
              <button @click="removeTodo(todo)" class="action-button delete">×</button>
            </div>
          </li>
        </TransitionGroup>
      </div>

      <div class="todo-column">
        <h2>Done ({{ doneTodos.length }})</h2>
        <TransitionGroup name="todo" tag="ul">
          <!-- 
            TODO: По БЭМ методологии нужно использовать модификатор через --
            Плохо: class="todo-item done"
            Хорошо: class="todo-item--done"
          -->
          <li 
            v-for="todo in doneTodos" 
            :key="todo.id"
            class="todo-item done"
          >
            <span>{{ todo.text }}</span>
            <div class="todo-actions">
              <button @click="returnTodo(todo)" class="action-button">↩</button>
              <button @click="removeTodo(todo)" class="action-button delete">×</button>
            </div>
          </li>
        </TransitionGroup>
      </div>
    </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const newTodoText = ref('');
const todos = ref([
  { id: 1, text: 'To study React fundamentals', done: false },
  { id: 2, text: 'To study React fundamentals', done: false },
  { id: 3, text: 'To study React fundamentals', done: false },
  { id: 4, text: 'To study React fundamentals', done: false },
  { id: 5, text: 'To study React fundamentals', done: true }
]);

const newTodos = computed(() => todos.value.filter(todo => !todo.done));
const doneTodos = computed(() => todos.value.filter(todo => todo.done));

let nextId = todos.value.length;

function addTodo() {
  if (newTodoText.value.trim() === '') return;
  
  todos.value.push({
    id: ++nextId,
    text: newTodoText.value.trim(),
    done: false
  });
  
  newTodoText.value = '';
}

function completeTodo(todo) {
  todo.done = true;
}

function returnTodo(todo) {
  todo.done = false;
}

function removeTodo(todo) {
  todos.value = todos.value.filter(t => t.id !== todo.id);
}
</script>

<!-- TODO: Добавить scoped для изоляции стилей -->
<style>
body {
  background-color: #C8DEC8;
}

h1 {
  font-weight: bold;
  font-size: 96px;
}

h2 {
  color: white;
  font-size: 19px;
}

h3 {
  font-size: 60px;
}
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
  border: 1px solid black;
  border-radius: 100px;
  background-color: #0D0714;
  overflow: hidden;
  position: fixed;
  bottom: -70px;
  left: 1000px;
  width: 983px;
  height: 800px;
}

.todo-app {
  color: #0D0714;
  font-weight: bold;
  font-size: 22px;
  background-color: #C8DEC8;
  margin-left: 250px;
}

.add-todo {
  display: flex;
  width: 450px;
  margin-bottom: 20px;
  margin-top: 80px;
}

.todo-input {
  flex: 1;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #3E1671;
  border-radius: 6px;
  margin-right: 10px;
  background-color: #0D0714;
  color: white;
}

.add-button {
  padding: 10px 15px;
  background: #9E78CF;
  color: rgb(15, 15, 15);
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 18px;
}

.todo-lists {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.todo-column {
  flex: 1;
  background: #0D0714;
  padding: 15px;
  border-radius: 8px;
  color: black;
  gap: 16px;
}

.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  margin: 8px 0;
  background: #15101C;
  border:1px solid #3E1671;
  border-radius: 9px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.1);
  color: #9E78CF;
}

/* TODO: Использовать БЭМ модификатор --done вместо .done */
.todo-item.done {
  opacity: 0.7;
}

.todo-item.done span {
  color: #42b983; 
  text-decoration: line-through;
}

.todo-actions {
  display: flex;
  gap: 5px;
}

.action-button {
  border: none;
  background: #15101C;
  color: #9E78CF;
  width: 25px;
  height: 25px;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.action-button.delete {
  color: #9E78CF;
}

/* Анимации */
.todo-enter-active,
.todo-leave-active {
  transition: all 0.5s ease;
}

.todo-enter-from,
.todo-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>
