<script setup>
import { Icon } from "@iconify/vue";
const props = defineProps({
  todoEl: Object,
  index: Number,
});

defineEmits(["toggleComplete", "editTodo", "updateTodo", "deleteTodo"]);
</script>

<template>
  <li>
    <input
      type="checkbox"
      :checked="todoEl.isCompleted"
      @input="$emit('toggleComplete', index)"
    >
    <div class="todo">
      <input
        v-if="todoEl.isEditing"
        type="text"
        :value="todoEl.todo"
        @input="$emit('updateTodo', index, $event.target.value)"
      >
      <span
        v-else
        :class="{'todo-complete': todoEl.isCompleted}"
      >
        {{ todoEl.todo }}
      </span>
    </div>
    <div class="todo-actions">
      <Icon
        v-if="todoEl.isEditing"
        class="icon"
        icon="system-uicons:check-circle-outside"
        color="#41b080"
        width="22"
        @click="$emit('editTodo', index)"
      />
      <Icon
        v-else
        class="icon"
        icon="ph:pencil-fill"
        color="#41b080"
        width="22"
        @click="$emit('editTodo', index)"
      />
      <Icon
        class="icon"
        icon="ph:trash"
        color="#f95e5e"
        width="22"
        @click="$emit('deleteTodo', todoEl.id)"
      />
    </div>
  </li>
</template>

<style lang="scss" scoped>
li {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 16px 10px;
  background-color: #f1f1f1;
  box-shadow: 0 20px 25px -5px rgb(0 0 0 / 0.1),
    0 8px 10px -6px rgb(0 0 0 / 0.1);

  &:hover {
    .todo-actions {
      opacity: 1;
    }
  }

  input[type="checkbox"] {
    appearance: none;
    width: 20px;
    height: 20px;
    background-color: #fff;
    border-radius: 50%;
    box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);

    &:checked {
      background-color: #41b080;
    }
  }

  .todo {
    flex: 1;

    input[type="text"] {
      width: 100%;
      padding: 2px 6px;
      border: 2px solid #41b080;
    }

    .todo-complete {
      text-decoration: line-through;
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