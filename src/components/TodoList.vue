<template>
  <div class="todoapp">
    <header class="header">
        <h1>vue todos</h1>
        <input
          class="new-todo"
          placeholder="What needs to be done?"
          type="text"
          v-model="text"
          v-on:keyup.enter="addItem($event.target.value)"
        />
    </header>

    <div class="main">
      <ul class="todo-list">
        <todo-item
          v-for="todo in filteredTodoItems"
          v-on:deleteItem="deleteItem"
          v-on:toggleItem="toggleItem"
          :todo="todo"
          :key="todo.id"
        />
      </ul>
    </div>

    <footer class="footer">
      <span class="todo-count">
        {{activeItemsCount}} items left
      </span>
      <ul class="filters">
        <li v-for="(type, index) in ['All', 'Active', 'Completed']" :key="index">
          <a
            v-on:click="setFilterType(index)"
            :class="getFilterClassName(index)"
          >
            {{type}}
          </a>
        </li>
      </ul>
    </footer>
  </div>
</template>

<script>
  import TodoItem from './TodoItem.vue'
  import uuid from '@/utils/uuid'

  const ALL = 0
  const ACTIVE = 1
  const COMPLETED = 2

  export default {
    props: ['todos'],
    components: {
      TodoItem
    },
    methods: {
      addItem: function (text) {
        if (text) {
          this.todoItems = this.todoItems.concat({
            id: uuid(),
            text,
            completed: false
          })
          // Clear the text input
          this.text = ''
        }
      },
      toggleItem: function (id) {
        this.todoItems = this.todoItems.map(item => {
          if (item.id === id) {
            return {
              ...item,
              completed: !item.completed
            }
          }
          return item
        })
      },
      deleteItem: function (id) {
        this.todoItems = this.todoItems.filter(item => item.id !== id)
      },
      setFilterType: function (type) {
        this.filterType = type
      },
      getFilterClassName: function (type) {
        return this.filterType === type ? 'selected' : ''
      }
    },
    data: function () {
      return {
        text: '',
        filterType: ALL,
        todoItems: []
      }
    },
    computed: {
      filteredTodoItems: function () {
        if (this.filterType === ACTIVE) {
          return this.todoItems.filter(item => !item.completed)
        } else if (this.filterType === COMPLETED) {
          return this.todoItems.filter(item => item.completed)
        }
        return this.todoItems.slice(0)
      },
      activeItemsCount: function () {
        return this.todoItems.filter(item => !item.completed).length
      }
    }
  }
</script>

<style scoped>
.filters a {
  cursor: pointer;
}
</style>
