<script setup lang="ts">
import TodoCreator, { type TodoState } from '@/components/TodoCreator.vue'
import { v4 as uuid } from 'uuid'
import { ref, type Ref } from 'vue'
import TodoItem from '@/components/TodoItem.vue'
import { Icon } from '@iconify/vue'

export interface TodoCreate {
  id: string
  todo: string
  isCompleted: boolean
  isEditing: boolean
}
const todoList: Ref<TodoCreate[]> = ref([])

function fetchTodoList() {
  const savedTodoList = JSON.parse(localStorage.getItem('todoList'))
  if (savedTodoList) {
    todoList.value = savedTodoList
  }
}

function setTodoListLocalStorage() {
  localStorage.setItem('todoList', JSON.stringify(todoList.value))
}

function createTodo(payload: TodoState) {
  payload.invalid = false

  if (payload.todo.length !== 0) {
    todoList.value.push({
      id: uuid(),
      todo: payload.todo,
      isCompleted: false,
      isEditing: false
    })
    payload.todo = ''
  } else {
    payload.invalid = true
    payload.errMsg = 'Todo value cannot be empty'
  }
  setTodoListLocalStorage()
}

function toggleTodoComplete(todoPos: number) {
  todoList.value[todoPos].isCompleted = !todoList.value[todoPos].isCompleted
  setTodoListLocalStorage()
}

function toggleTodo(todoPos: number) {
  todoList.value[todoPos].isEditing = !todoList.value[todoPos].isEditing
  setTodoListLocalStorage()
}

function updateTodo(value: string, todoPos: number) {
  todoList.value[todoPos].todo = value
  setTodoListLocalStorage()
}

function deleteTodo(id: string) {
  todoList.value = todoList.value.filter((todo) => todo.id !== id)
  setTodoListLocalStorage()
}
</script>

<template>
  <main>
    <h2 class="title">Create Todo</h2>
    <TodoCreator @create-todo="(todo) => createTodo(todo)" />
    <ul class="todo-list" v-if="todoList.length > 0">
      <TodoItem
        v-for="(todo, index) in todoList"
        :key="todo.id"
        :todo="todo"
        :index="index"
        @toggle-complete="(indexValue) => toggleTodoComplete(indexValue)"
        @edit-todo="(indexValue) => toggleTodo(indexValue)"
        @update-todo="(value, indexValue) => updateTodo(value, indexValue)"
        @delete-todo="(id) => deleteTodo(id)"
      />
    </ul>
    <p class="todos-msg" v-else>
      <Icon icon="noto-v1:sad-but-relieved-face" color="#41b080" width="22" />
      <span>You have no todo's to complete! Add one!</span>
    </p>
  </main>
</template>

<style lang="scss" scoped>
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }

  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>
