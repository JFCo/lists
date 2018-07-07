<template>
<div class="container is-mobile">
<div class="column is-4">
<!-- <div class="column is-4"> -->
<nav class="panel">
<p class="panel-heading"><b>List Maker</b>
</p>
<div class="panel-block">
<div class="control is-expanded">
<inputText
v-model="newTodoText"
@keydown.enter="addTodo"/>
<div v-if="todos.length">
<listItem
v-for="todo in todos"
:key="todo.id"
:todo="todo"
@remove="removeTodo"/>
</div>
<div class="control is-expanded" v-else>
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
    //   currentOrder: 'text'
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
    // toggleOrder (text) {
    //   this.currentOrder = text
    // },
    // orderedItems (todos) {
    //   return todos.filter(todo => {
    //     if (todo[this.currentOrder]) return todo
    //   })
    // }
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
    // sortedTodos () {
    //   console.log('[todos] sortedTodos changed')
    // }
  }
}
</script>
<style scoped>
.panel {margin-top: .1rem}
</style>
