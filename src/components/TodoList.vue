<template>
  <ul class="todo-list">
    <slot />
  </ul>
</template>

<script>
export default {
  name: 'TodoList'
}
</script>

<style lang="scss">
  .todo-list {
    list-style: none;
    padding: 0;
    margin: 0;
  }
</style>
