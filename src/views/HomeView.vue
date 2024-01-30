<script setup>
import { onMounted, ref, watch, computed } from "vue";
import { uid } from "uid";
import { Icon } from "@iconify/vue";
import TodoCreater from "../components/TodoCreater.vue";
import TodoItem from "../components/TodoItem.vue";

const todoList = ref([]);

const todoCompleted = computed(() => {
  return todoList.value.every((todo) => todo.isCompleted);
});

const featchTodoList = () => {
  const savedTodoList = JSON.parse(localStorage.getItem("todoList"));
  if (savedTodoList) {
    todoList.value = savedTodoList;
  }
};

const setTodoListLocalStorage = () => {
  localStorage.setItem("todoList", JSON.stringify(todoList.value));
};

const createTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: false,
    isEditing: false,
  });
};

const toggleTodoComplete = (todoPos) => {
  todoList.value[todoPos].isCompleted = !todoList.value[todoPos].isCompleted;
};

const toggleEditTodo = (todoPos) => {
  todoList.value[todoPos].isEditing = !todoList.value[todoPos].isEditing;
};

const EditTodoValue = (todoPos, todoVal) => {
  todoList.value[todoPos].todo = todoVal;
};

const deleteTodo = (todoId) => {
  todoList.value = todoList.value.filter((todo) => todo.id !== todoId);
};

watch(
  todoList,
  () => {
    console.log(123);
    setTodoListLocalStorage();
  },
  {
    deep: true,
  }
);

onMounted(() => {
  featchTodoList();
});
</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreater @createTodo="createTodo" />
    <ul
      class="todo-list"
      v-if="todoList.length !== 0"
    >
      <TodoItem
        v-for="(todoEl, index) in todoList"
        :key="index"
        :todoEl="todoEl"
        :index="index"
        @toggle-complete="toggleTodoComplete"
        @edit-todo="toggleEditTodo"
        @update-todo="EditTodoValue"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p
      class="todos-msg"
      v-else
    >
      <Icon
        icon="noto-v1:crying-cat"
        width="35"
      />
      <span>You have no todo's to complete! Add one!</span>
    </p>
    <p
      v-if="todoCompleted && todoList.length > 0"
      class="todos-msg"
    >
      <Icon
        icon="bxs:happy"
        width="45"
        style="color: #41b080"
      />
      <span>You have completed all your todos!</span>
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
