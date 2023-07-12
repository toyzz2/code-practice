<template>
  <div class="input-warp" :class="{ 'input-err': todoState.invalid }">
    <input type="text" v-model="todoState.todo" />
    <TodoButton @click="createTodo" @keyup.enter.native="s"> </TodoButton>
  </div>
  <p v-if="todoState.invalid" class="err-msg">{{ todoState.errMsg }}</p>
</template>

<script setup>
import { ref, reactive } from 'vue'

import TodoButton from './TodoButton.vue'

const emit = defineEmits(['create-todo'])
const todoState = reactive({
  todo: '',
  invalid: null,
  errMsg: ''
})

const s = ()=>{
  console.log(111);
}

const createTodo = () => {
  todoState.invalid = null
  if (todoState.todo !== '') {
    emit('create-todo', todoState.todo)
    todoState.todo = ''
    return
  }

  todoState.invalid = true
  todoState.errMsg = 'todo value cannot be empty'
}
</script>
)

<style lang="scss" scoped>
.input-warp {
  display: flex;
  transition: 250ms ease;
  border: 2px solid #41b080;
  &.input-err {
    border-color: red;
  }

  &:focus-within {
    box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1), 0 -2px 4px -2px rgb(0 0 0 / 0.1);
  }

  input {
    width: 100%;
    padding: 8px 6px;
    border: none;

    &:focus {
      outline: none;
    }
  }
}

.err-msg {
  margin-top: 6px;
  font-style: 12px;
  text-align: center;
  color: red;
}
</style>
