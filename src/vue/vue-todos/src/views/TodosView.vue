<script setup>
import { uid } from 'uid'
import { Icon } from '@iconify/vue'
import { ref, computed } from 'vue'
import TodoCreator from '../components/TodoCreator.vue'
import TodoItem from '../components/TodoItem.vue'

const todoList = ref([])

// 是否全部完成
const todosCompleted = computed(() => {
  return todoList.value.every((t) => t.isCompleted)
})

// 从localStorage获取todoList
const fetchTodoList = () => {
  const savedTodoList = JSON.parse(localStorage.getItem('todoList'))
  savedTodoList && (todoList.value = savedTodoList)
}
fetchTodoList()

// 保存todoList到localStorage
const setTodoListLocalStorage = () => {
  localStorage.setItem('todoList', JSON.stringify(todoList.value))
}

// 创建todo
const createTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: null,
    isEditing: null
  })
  setTodoListLocalStorage()
}
// 切换todo编辑状态
const toggleEditTodo = (todo) => {
  todoList.value.find((t) => t.id === todo.id).isEditing = !todo.isEditing
  setTodoListLocalStorage()
}

// 完成todo
const toggleTodoComplete = (todo) => {
  todoList.value.find((t) => t.id === todo.id).isCompleted = !todo.isCompleted
  setTodoListLocalStorage()
}
// 更新todo
const updateTodo = (todoVal, todo) => {
  todoList.value.find((t) => t.id === todo.id).todo = todoVal
  setTodoListLocalStorage()
}
// 删除todo
const deleteTodo = (todo) => {
  todoList.value = todoList.value.filter((t) => t.id !== todo.id)
  setTodoListLocalStorage()
}
</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodo" />

    <ul class="todo-list" v-if="todoList.length > 0">
      <TodoItem
        v-for="(todo, index) in todoList"
        :key="todo.id"
        :todo="todo"
        @edit-todo="toggleEditTodo"
        @update-todo="updateTodo"
        @toggle-complete="toggleTodoComplete"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p class="todos-msg" v-else>
      <Icon icon="noto-v1:sad-but-relieved-face" />
      <span>You have no todo's to complete! Add one!</span>
    </p>
    <p v-if="todosCompleted && todoList.length > 0" class="todos-msg">
      <Icon icon="noto-v1:party-popper" />
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
    text-align: center;
    margin-bottom: 16px;
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
