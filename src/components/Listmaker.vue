<template>
<div class="container is-fluid">
<!-- <div class="columns"> -->
<div class="column is-6">
<nav class="panel">
<p class="panel-heading">List Maker</p>
<div class="panel-block">
<div class="field">
<inputText
v-model="newTodoText"
placeholder="New todo"
@keydown.enter="addTodo"/>
<ul v-if="todos.length">
<listItem
v-for="todo in todos"
:key="todo.id"
:todo="todo"
@remove="removeTodo"/>
</ul>
<div class="field-body" v-else>
Nothing left in the list. Add a new todo in the input above.
</div>
</div>
</div>
</nav>
</div>
</div>
<!-- </div> -->
</template>

<script>
import inputText from './inputText.vue'
import listItem from './listItem.vue'

let nextTodoId = 1

export default {
  components: {
    inputText, listItem
  },
  data () {
    return {
      newTodoText: '',
      todos: []
    }
  },
  methods: {
    addTodo () {
      const trimmedText = this.newTodoText.trim()
      if (trimmedText) {
        this.todos.push({
          id: nextTodoId++,
          text: trimmedText
        })
        this.newTodoText = ''
      }
    },
    removeTodo (idToRemove) {
      this.todos = this.todos.filter(todo => {
        return todo.id !== idToRemove
      })
    }
  },
  mounted () {
    console.log('App mounted!')
    if (localStorage.getItem('todos')) this.todos = JSON.parse(localStorage.getItem('todos'))
  },
  watch: {
    todos: {
      handler () {
        console.log('Todos changed!')
        localStorage.setItem('todos', JSON.stringify(this.todos))
      },
      deep: true
    }
  }
}
</script>
