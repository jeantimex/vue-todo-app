<template>
  <li
    class="todo-item"
    :class="{completed: todo.completed}"
  >
    <div class="view">
      <input
        class="toggle"
        type="checkbox"
        :id="todo.id"
        :checked="todo.completed"
        v-on:change="toggleItem(todo.id)"
      />
      <label :for="todo.id">
        {{todo.text}}
      </label>
      <button
        class="destroy"
        v-on:click="deleteItem(todo.id)"
      />
    </div>
  </li>
</template>

<script>
  export default {
    name: 'todo-item',
    props: ['todo'],
    methods: {
      deleteItem: function (id) {
        this.$emit('deleteItem', id)
      },
      toggleItem: function (id) {
        this.$emit('toggleItem', id)
      }
    }
  }
</script>
