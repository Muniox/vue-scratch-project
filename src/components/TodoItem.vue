<script setup lang="ts">
import type { TodoCreate } from '@/views/TodosView.vue'
import { Icon } from '@iconify/vue'

const props = defineProps<{
  todo: TodoCreate
  index: number
}>()

defineEmits<{
  'toggle-complete': [index: number]
  'edit-todo': [index: number]
  'update-todo': [value: string, index: number]
}>()
</script>

<template>
  <li>
    <input
      type="checkbox"
      :checked="props.todo.isCompleted"
      @input="$emit('toggle-complete', props.index)"
    />
    <div class="todo">
      <input
        v-if="props.todo.isEditing"
        type="text"
        :value="props.todo.todo"
        @input="$emit('update-todo', ($event.target as HTMLInputElement).value, index)"
      />
      <span v-else :class="{ 'completed-todo': props.todo.isCompleted }">
        {{ props.todo.todo }}</span
      >
    </div>
    <div class="todo-actions">
      <Icon
        v-if="props.todo.isEditing"
        icon="ph:check-circle"
        color="#41b080"
        width="22"
        class="icon"
        @click="$emit('edit-todo', index)"
      />
      <Icon
        v-else
        icon="ph:pencil-fill"
        color="#41b080"
        width="22"
        class="icon"
        @click="$emit('edit-todo', index)"
      />
      <Icon icon="ph:trash" color="#f95e5e" width="22" class="icon" />
    </div>
  </li>
</template>

<style scoped lang="scss">
li {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 16px 10px;
  background-color: #f1f1f1;
  box-shadow:
    0 20px 25px -5px rgb(0 0 0 / 0.1),
    0 8px 10px -6px rgb(0 0 0 / 0.1);

  &:hover {
    .todo-actions {
      opacity: 1;
    }
  }

  input[type='checkbox'] {
    appearance: none;
    width: 20px;
    height: 20px;
    background-color: #fff;
    border-radius: 50%;
    box-shadow:
      0 4px 6px -1px rgb(0 0 0 / 0.1),
      0 2px 4px -2px rgb(0 0 0 / 0.1);

    &:checked {
      background-color: #41b080;
    }
  }

  .todo {
    flex: 1;

    .completed-todo {
      text-decoration: line-through;
    }

    input[type='text'] {
      width: 100%;
      padding: 2px 6px;
      border: 2px solid #41b080;
    }
  }

  .todo-actions {
    display: flex;
    gap: 6px;
    opacity: 0;
    transition: 150ms ease-in-out;
    .icon {
      cursor: pointer;
    }
  }
}
</style>
