<template>
  <section class="real-app">
    <input type="text" class="add-input" autofocus="autofocus" placeholder="接下去要做什么？" @keyup.enter="addTodo">
    <Item 
    :todo="todo"
    v-for="todo in filteredTodos"
    :key="todo.id"
    @del="deleteTodo"
    />
    <Tabs 
    :filter="filter"
    :todos="todos"
    @toggle="toggleFilter"
    @clearAllCompleted="clearAllCompleted"
    />
  </section>
</template>

<script>
import Item from './items'
import Tabs from './tabs'
let id = 0
export default {
  data() {
    return{
      todos: [],
      filter: 'all'
    }
  },
  components: {
    Item,
    Tabs
  },
  computed: {
    filteredTodos() {
      if(this.filter === 'all') {
        return this.todos
      }
      const completed = this.filter === 'completed'
      return this.todos.filter(todo => completed === todo.completed)
    }
  },
  methods: {
    addTodo(e) {
      if(e.target.value.trim() != "") {
        this.todos.unshift({
          id: id++,
          content: e.target.value.trim(),
          completed: false
        })
        e.target.value = ''
      }else {
        this.todo = null
      }
    },
    deleteTodo(id) {
      this.todos.splice(this.todos.findIndex(todo => todo.id === id),1)
    },
    toggleFilter(state) {
      this.filter = state
    },
    clearAllCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  }
}
</script>

<style scoped>
.real-app {
  width: 600px;
  margin: 0 auto;
  box-shadow: 0 0 5px #666;
  opacity: 0.85;
  border-radius: 10px;
}
.add-input {
  position: relative;
  margin: 0;
  width: 100%;
  font-size: 24px;
  font-family: inherit;
  font-weight: inherit;
  line-height: 1.4em;
  border: 0;
  outline: none;
  color: inherit;
  box-sizing: border-box;
  font-smoothing: antialiased;
  padding: 16px 16px 16px 36px;
  border: none;
  box-shadow: inset 0 -2px 1px rgba(0, 0, 0, 0.03);
  border-top-right-radius: 10px;
  border-top-left-radius: 10px;
}
</style>
