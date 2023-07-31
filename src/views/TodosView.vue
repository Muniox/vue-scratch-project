<script setup lang="ts">
import TodoCreator, { type TodoState } from '@/components/TodoCreator.vue'
import { v4 as uuid } from 'uuid'
import { ref, type Ref } from 'vue'

interface TodoCreate {
  id: string
  todo: string
  isCompleted: boolean | null
  isEditing: boolean | null
}

const todoList: Ref<TodoCreate[]> = ref([])

function createTodo(payload: TodoState) {
  payload.invalid = false

  if (payload.todo.length !== 0) {
    todoList.value.push({
      id: uuid(),
      todo: payload.todo,
      isCompleted: null,
      isEditing: null
    })
    payload.todo = ''
  } else {
    payload.invalid = true
    payload.errMsg = 'Todo value cannot be empty'
  }
}
</script>

<template>
  <main>
    <h2 class="title">Create Todo</h2>
    <TodoCreator @create-todo="(todo) => createTodo(todo)" />
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

  .title {
    margin-bottom: 16px;
    text-align: center;
  }
}
</style>
