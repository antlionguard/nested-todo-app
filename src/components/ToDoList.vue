<template>
  <div class="wrapper">
    <ul class="list">
      <li class="item" v-for="(todo, index) in todos" :key="index">
        <p :class="{ 'is-selected-for-create': todo.isSelectedForCreate }">
          <span
            class="item-text"
            :class="{ 'is-done': todo.isDone }"
            @click="setDone(todo)"
            >{{ todo.name }}</span
          >
          <span class="folder" v-if="todo.todos.length" @click="todo.isOpen = !todo.isOpen"> [{{ todo.isOpen ? '-' : '+' }}] </span>
          <span class="remove" @click="removeTodo(todos, index)">Remove</span>
        </p>
        <ToDoList
          v-if="todo.todos && todo.isOpen"
          :todos="todo.todos"
          @setCreateToDoItem="($event) => $emit('setCreateToDoItem', $event)"
        />
        <span class="add-new-todo" @click="$emit('setCreateToDoItem', todo)"
          >Add new todo</span
        >
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'ToDoList',
  emits: ['setCreateToDoItem'],
  props: {
    todos: {
      type: Array,
      required: true,
    }
  },
  setup (_, { emit }) {
    function removeTodo (array, index) {
      array.splice(index, 1)
      emit('setCreateToDoItem', null)
    }
    function setDone (todo) {
      todo.isDone = !todo.isDone
      if (todo.todos) {
        setDoneAllChilds(todo, todo.isDone)
      }
    }
    function setDoneAllChilds (todo, isDone) {
      todo.todos.forEach(element => {
        element.isDone = isDone
        if (element.todos) {
          setDoneAllChilds(element, isDone)
        }
      });
    }
    return {
      removeTodo,
      setDone
    }
  }
}
</script>

<style>
.wrapper {
  text-align: left;
}
.list {
  margin-bottom: 30px;
}
.remove {
  text-decoration: underline;
  cursor: pointer;
}
.add-new-todo {
  cursor: pointer;
  text-decoration: underline;
  margin-top: -15px;
  display: block;
}
.item-text {
  cursor: pointer;
}
.is-selected-for-create {
  font-weight: bold;
}
.is-done {
  text-decoration: line-through;
}
.folder {
  cursor: pointer;
  font-weight: bold;
}
</style>