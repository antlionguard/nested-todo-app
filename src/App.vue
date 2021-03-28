<template>
  <div>
    <CreateToDo @createToDo="createToDo" />
    <ToDoList :todos="todos" @setCreateToDoItem="setToDoForCreate" />
    <span class="add-new-todo" @click="setToDoForCreate(null)">Add new todo</span>

  </div>
</template>

<script>
import { ref } from 'vue'
import CreateToDo from './components/CreateToDo'
import ToDoList from './components/ToDoList'

export default {
  name: 'App',
  components: {
    CreateToDo,
    ToDoList
  },
  setup () {
    // Dummy TODO List
    const todos = ref([
      {
        name: 'Vue 3 Composition API Öğren',
        todos: [
          {
            name: 'Setup nedir?',
            todos: []
          },
          {
            name: 'Ref ve Reactive nedir?',
            todos: []
          }
        ],
        isSelectedForCreate: false,
        isDone: false,
        isOpen: false,
      },
      {
        name: 'Buna ad bulamadım ama altında eleman olmayacak.',
        todos: [],
        isSelectedForCreate: false,
        isDone: false,
        isOpen: false,
      },
      {
        name: 'Buna da ad bulamadım ama bunun da altında eleman olmayacak.',
        todos: [],
        isSelectedForCreate: false,
        isDone: false,
        isOpen: false,
      }
    ])
    // TODO For Create
    let toDoForCreate = ref(null)
    async function setToDoForCreate (todo) {
      await setAllItemsUnSelected(todos.value)
      if (todo) {
        todo.isSelectedForCreate = true
      }
      toDoForCreate.value = todo
    }
    function setAllItemsUnSelected (items) {
      items.forEach(element => {
        element.isSelectedForCreate = false
        if (element.todos) {
          setAllItemsUnSelected(element.todos)
        }
      });
    }
    // Create TODO
    function createToDo (todo) {
      if (!toDoForCreate.value) {
        todos.value.push(todo)
      } else {
        toDoForCreate.value.todos.push(todo)
      }
    }
    return { todos, toDoForCreate, setToDoForCreate, createToDo }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
