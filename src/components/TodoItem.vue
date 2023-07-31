<script setup lang="ts">
import type { TodoCreate } from '@/views/TodosView.vue'

const props = defineProps<{
  todo: TodoCreate
}>()
</script>

<template>
  <li>
    <input type="checkbox" :checked="props.todo.isCompleted" />
    <div class="todo">
      <input type="text" :value="props.todo.todo" />
    </div>
  </li>
</template>

<style scoped lang="scss"></style>
