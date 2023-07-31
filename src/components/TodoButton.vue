<script setup lang="ts">
export interface TodoState {
  todo: string
  invalid: boolean
  errMsg: string
}

const props = defineProps<{
  todoState: TodoState
}>()

const emit = defineEmits<{
  'create-todo': [todo: TodoState]
}>()

function createTodo() {
  emit('create-todo', props.todoState)
}
</script>

<template>
  <button @click="createTodo()" :todoState="todoState">Create</button>
</template>

<style scoped lang="scss"></style>
