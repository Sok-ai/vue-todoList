<script setup>
import { reactive } from "vue";

const todoState = reactive({
  todo: "",
  invalid: false,
  errMsg: "",
});

const emit = defineEmits(["create-todo"]);

const validTodo = () => {
  if (todoState.todo) {
    emit("create-todo", todoState.todo);
    todoState.todo = "";
    todoState.invalid = false;
  } else {
    todoState.invalid = true;
    todoState.errMsg = "Todo value cannot be empty";
  }
};
</script>

<template>
  <div>
    <div
      class="input-wrap"
      :class="{ 'input-err': todoState.invalid }"
    >
      <input
        type="text"
        v-model="todoState.todo"
      >
      <button @click="validTodo">Create</button>
    </div>
    <p
      class="err-msg"
      v-show="todoState.invalid"
    >{{ todoState.errMsg }}</p>
  </div>
</template>

<style lang="scss" scoped>
.input-wrap {
  display: flex;
  transition: 250ms ease;
  border: 2px solid #41b080;

  &.input-err {
    border-color: red;
  }

  &:focus-within {
    box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1),
      0 -2px 4px -2px rgb(0 0 0 / 0.1);
  }

  input {
    width: 100%;
    padding: 8px 6px;
    border: none;

    &:focus {
      outline: none;
    }
  }

  button {
    padding: 8px 16px;
    border: none;
    cursor: pointer;
    &:hover {
      background: #c2c2c2;
    }
  }
}

.err-msg {
  margin-top: 6px;
  font-size: 12px;
  text-align: center;
  color: red;
}
</style>