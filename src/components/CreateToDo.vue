<template>
  <div class="wrapper">
      <form @submit.prevent="createToDo">
        <label> ToDo </label>
        <input v-model="name" />
        <button> Add new todo </button>
      </form>
  </div>
</template>

<script>
import { ref } from 'vue'
export default {
  emits: ['createToDo'],
  setup (_, context) {
    const name = ref('')
    function createToDo () {
      if (!name.value) {
        return
      }
      context.emit('createToDo', {
        name: name.value,
        todos: [],
        isSelectedForCreate: false,
        isDone: false,
        isOpen: true,
      })
      name.value = ''
    }
    return { name, createToDo }
  }
}
</script>

<style>
</style>