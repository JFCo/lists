<template>
<div class="container is-fluid">
<div class="columns">
  <div class="column">
<nav class="panel">
  <p class="panel-heading">{{ today.day }} {{ today.date }}</p>
  <div class="panel-block">
      <form @keydown.enter.prevent>
        <b-field v-bind:class="{ active: new_todo }">
            <b-input placeholder="Add to the List"
                v-model="new_todo"
                v-on:keyup.enter="addItem"
                type="input"
                icon="fa fa-plus">
            </b-input>
            <p class="control">
                <button class="button is-primary" @click="addItem">Add to List</button>
            </p>
        </b-field>
        <!-- <p class="control has-icons-left is-expanded">
        <input class="input " v-bind:class="{ active: new_todo }" placeholder="Add to the List!" v-model="new_todo" v-on:keyup.enter="addItem">
        <span class="icon is-small is-left" v-bind:class="{ active: new_todo }"  @click="addItem">
        <i class="fas fa-plus" aria-hidden="true"></i>
        </span>
        </p> -->
      </form>
  </div>
    <div class="panel-block" v-if="pending.length > 0">
        <b-tag class="is-warning">You have {{ pending.length }} pending item<span v-if="todoList.length>1">s</span></b-tag>
          <b-field horizontal v-for="item in pending" v-bind:key="item.title">
            <b-checkbox class="checkbox" v-bind:id="'item_' + item.id" v-model="item.done" type="checkbox"></b-checkbox>
            <b-field v-bind:for="'item_' + item.id"></b-field>
            <b-field>{{ item.title }}</b-field>
            <b-field>
            <a class="button is-danger" @click="deleteItem(item)">RM</a>
            </b-field>
          </b-field>
    </div>
<div class="panel-block" v-if="!pending.length">
        <p class="panel-heading">Time to chill! You have no to-dos.</p>
</div>
<div class="panel-block" v-if="completed.length > 0 && showComplete">
        <b-field label="Completed tasks" expanded>{{ completedPercentage }}</b-field>
        <!-- <transition-group name="todo-item" tag="ul" class="field"> -->
          <b-field expanded v-for="item in completed" v-bind:key="item.title">
            <b-checkbox class="checkbox" v-bind:id="'item_' + item.id" v-model="item.done" type="checkbox"></b-checkbox>
            <label v-bind:for="'item_' + item.id"></label>
            <b-field>{{ item.title }} <a class="button is-danger" @click="deleteItem(item)">Remove</a></b-field>
          </b-field>
        <!-- </transition-group> -->
</div>
<b-field>
        <button class="button is-success" v-if="completed.length > 0" @click="toggleShowComplete"> <span style="margin-right:8px;" v-if="!showComplete">Show </span><span style="margin-right:8px;" v-else>Hide</span>Complete</button>
        <button class="button is-danger" v-if="todoList.length > 0" @click="clearAll">Clear All</button>
</b-field>
  <!-- <footer>
    <p>Footer Stuff</p>
  </footer> -->
</nav>
</div>
</div>
</div>
</template>

<script>

export default {
  data () {
    return {
      todoList: [
        {'id': 0, 'title': 'one', 'done': false},
        {'id': 1, 'title': 'two', 'done': false},
        {'id': 4, 'title': 'three', 'done': true}
      ],
      new_todo: '',
      showComplete: false
    }
  },
  mounted () {
    this.getTodos()
  },
  watch: {
    todoList: {
      handler: function (updatedList) {
        localStorage.setItem('todo_list', JSON.stringify(updatedList))
      },
      deep: true
    }
  },
  computed: {
    pending () {
      return this.todoList.filter(function (item) {
        return !item.done
      })
    },
    completed () {
      return this.todoList.filter(function (item) {
        return item.done
      })
    },
    completedPercentage () {
      return (Math.floor((this.completed.length / this.todoList.length) * 100)) + '%'
    },
    today () {
      var weekday = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']
      var today = new Date()
      var dd = today.getDate()
      var mm = today.getMonth() + 1 // January is 0!
      var yyyy = today.getFullYear()

      if (dd < 10) {
        dd = '0' + dd
      }

      if (mm < 10) {
        mm = '0' + mm
      }

      today = {
        day: weekday[today.getDay()],
        date: mm + '-' + dd + '-' + yyyy
      }

      return (today)
    }
  },
  methods: {
    // get all todos when loading the page
    getTodos () {
      if (localStorage.getItem('todo_list')) {
        this.todoList = JSON.parse(localStorage.getItem('todo_list'))
      }
    },
    // add a new item
    addItem () {
      // validation check
      if (this.new_todo) {
        this.todoList.unshift({
          id: this.todoList.length,
          title: this.new_todo,
          done: false
        })
      }
      // reset new_todo
      this.new_todo = ''
      // save the new item in localstorage
      return true
    },
    deleteItem (item) {
      this.todoList.splice(this.todoList.indexOf(item), 1)
    },
    toggleShowComplete () {
      this.showComplete = !this.showComplete
    },
    clearAll () {
      this.todoList = []
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
