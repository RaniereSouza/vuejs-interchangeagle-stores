<script setup lang="ts">
  import { ref, computed, onMounted } from 'vue'

  const title = 'TODO App'

  const todoList = ref([
    {description: 'Study Vue.js',         isDone: true },
    {description: 'Do laundry',           isDone: false},
    {description: 'Do dishes',            isDone: false},
    {description: 'Take the garbage out', isDone: false},
    {description: 'Feed the cats',        isDone: false},
  ])
  const newTodoItem = ref('')

  const orderedTodoList = computed(
    () => [...todoList.value].sort(item => item.isDone ? -1 : 1)
  )
  const tasksTotal = computed(
    () => todoList.value.length
  )
  const tasksDone = computed(
    () => todoList.value.filter(item => item.isDone).length
  )
  const todoDescription = computed(
    () => `Completed: ${tasksDone.value} out of ${tasksTotal.value}`
  )

  onMounted(() => {
    document.title = title
  })

  function addNewTodoItem() {
    todoList.value.push({description: newTodoItem.value, isDone: false})
    newTodoItem.value = ''
  }
</script>

<template>
  <header class="todo-header">
    <h1 class="todo-page-title">{{ title }}</h1>
    <p
      class="todo-description"
      :class="{'all-done': tasksDone === tasksTotal}"
    >
      {{ todoDescription }}
    </p>
  </header>

  <ul class="todo-list">
    <li
      v-for="todoItem of orderedTodoList"
      class="todo-item"
      :class="{'is-done': todoItem.isDone}"
    >
      <span class="material-icons" @click="todoItem.isDone = !todoItem.isDone">
        {{ todoItem.isDone ? 'check_box' : 'check_box_outline_blank' }}
      </span>
      {{ todoItem.description }}
    </li>

    <li class="todo-item">
      <input
        type="text"
        class="new-todo-item"
        placeholder="Insert a new task here"
        v-model="newTodoItem"
        @keyup.enter="addNewTodoItem"
      />
      <button
        class="add-new-todo-item"
        :disabled="!newTodoItem"
        @click="addNewTodoItem"
      >
        Add
      </button>
    </li>
  </ul>
</template>

<style scoped>
  .todo-header, .todo-list {
    width: min(90%, 720px);
  }

  .todo-header {
    text-align: center;
  }

  .todo-page-title {
    color: cornflowerblue;
  }

  .todo-description.all-done {
    color: green;
  }

  .todo-list {
    margin-top: 1rem;
    list-style: none;
  }

  .todo-item + .todo-item {
    margin-top: .5rem;
  }

  .todo-item {
    display: flex;
    align-items: center;
    justify-content: flex-start;
    gap: .5rem;
    padding: .5rem .75rem;
    border-radius: .25rem;
    border: 1px solid #ccc;
    border-left: 8px solid #ccc;
  }

  .todo-item:has(.new-todo-item) {
    justify-content: space-between;
  }

  .todo-item .material-icons {
    color: #ccc;
    font-size: 1rem;
    cursor: pointer;
  }

  .todo-item.is-done {
    border-color: burlywood;
  }

  .todo-item.is-done .material-icons {
    color: burlywood;
  }

  .new-todo-item {
    border: none;
    outline: none;
    width: 100%;
  }

  .add-new-todo-item {
    margin: -0.5rem -0.75rem -0.5rem 0;
    border-radius: 0 .25rem .25rem 0;
    border: none;
    padding: .5rem .75rem;
    background-color: #ccc;
  }

  .add-new-todo-item:disabled {
    opacity: .5;
  }
</style>
