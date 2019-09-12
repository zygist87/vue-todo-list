<template>
  <div class="app">
    <div class="surface">
      <header class="surface__header">
        <h1 class="headline">
          <i class="material-icons">
            assignment_turned_in
          </i>
          Todo List
        </h1>
        <div class="surface__filters">
          <BaseButton
            v-for="filter in filters"
            :key="filter"
            :outlined="filter !== activeFilter"
            class="surface__filter"
            @click="addFilter(filter)"
          >
            {{ filter }}
          </BaseButton>
          <BaseButton
            v-for="type in sortTypes"
            :key="type"
            :outlined="type !== sortedBy"
            class="surface__filter"
            @click="setSortedBy(type)"
          >
            {{ type }}
          </BaseButton>
        </div>
      </header>
      <h4 class="headline headline--4"> {{ today }} </h4>

      <div class="surface__new">
        <BaseTextField
          v-model="newTodoText"
          placeholder="What needs to be done?"
          class="surface__input"
          @keydown.enter="handleClick"
        />
        <BaseButton
          class="surface__button"
          @click="handleClick"
        >
            <i class="material-icons"> playlist_add </i>
        </BaseButton>
      </div>

      <div
        v-if="todos.length > 0"
        class="surface__todos"
      >
        <TodoList>
          <TodoListItem
            v-for="todo in filteredTodos"
            :key="todo.id"
            :todo="todo"
            @change="handleChange(todo.id, $event)"
            @delete="handleDelete(todo.id)"
          />
        </TodoList>
      </div>
    </div>

  </div>
</template>

<script>
import uniqid from 'uniqid'
import BaseButton from '@/components/BaseButton'
import BaseTextField from '@/components/BaseTextField'
import TodoList from '@/components/TodoList'
import TodoListItem from '@/components/TodoListItem'

export default {
  name: 'App',
  components: {
    BaseButton,
    BaseTextField,
    TodoList,
    TodoListItem
  },
  data () {
    return {
      activeFilter: 'all',
      sortedBy: 'asc',
      sortTypes: ['asc', 'desc'],
      filters: ['all', 'completed', 'in-progress'],
      newTodoText: null,
      todos: []
    }
  },
  computed: {
    filteredTodos () {
      const filteredTodos = this.todos.filter(todo => {
        switch (this.activeFilter) {
          case 'all':
            return true
          case 'completed':
            return todo.done
          case 'in-progress':
            return !todo.done
          default:
            return true
        }
      })

      if (this.sortedBy === 'asc') {
        return filteredTodos.sort((a, b) => a.time - b.time)
      }

      if (this.sortedBy === 'desc') {
        return filteredTodos.sort((a, b) => b.time - a.time)
      }

      return filteredTodos
    },
    today () {
      return new Date().toLocaleString('en-GB', { weekday: 'long', month: 'long', day: '2-digit' })
    }
  },

  methods: {
    addFilter (value) {
      this.activeFilter = value
    },
    setSortedBy (type) {
      this.sortedBy = type
    },
    handleClick (event) {
      if (!this.newTodoText) return
      const todo = {
        id: uniqid(),
        time: Date.now(),
        text: this.newTodoText,
        done: false
      }
      this.todos = [...this.todos, todo]
      this.newTodoText = null
    },

    handleChange (id, event) {
      this.todos = this.todos.map(todo => {
        if (todo.id === id) {
          todo.done = event.target.checked
        }
        return todo
      })
    },

    handleDelete (id) {
      this.todos = this.todos
        .filter(todo => todo.id !== id)
    }
  }
}
</script>

<style lang="scss">
  .app {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100%;
    padding: 1.5rem;
  }

  .headline {
    display: flex;
    align-items: center;
    font-size: 1.75rem;
    margin: 0;
    margin-bottom: 1.5rem;

    &--4 {
      font-size: 1.15rem;
      font-weight: normal;
      color: rgba(black, 0.51);
    }

    .material-icons {
      font-size: 2.5rem;
      margin-right: 0.5rem;
    }
  }

  .surface {
    background-color: white;
    padding: 2rem;
    border-radius: 4px;
    box-shadow: 0px 1px 3px 0px rgba(0, 0, 0, 0.1);
    max-width: 600px;
    width: 100%;

    &__new {
      display: flex;
      margin-bottom: 1.5rem;
    }

    &__input {
      border-top-right-radius: 0;
      border-bottom-right-radius: 0;
      border-right: 0;
    }

    &__button {
      border-top-left-radius: 0;
      border-bottom-left-radius: 0;
    }

    &__header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 1.5rem;

      .headline {
        margin-bottom: 0;
      }
    }

    &__filters {
      margin: 0 -0.75rem;
    }
    &__filter {
      margin: 0 0.75rem;
    }
  }
</style>
