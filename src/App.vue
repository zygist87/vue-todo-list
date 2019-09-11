<template>
  <div class="app">
    <div class="surface">
      <h1 class="headline"> Todo List </h1>
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
            <i class="material-icons"> add </i>
        </BaseButton>
      </div>

      <div class="surface__todos">
        <TodoList>
          <TodoListItem
            v-for="todo in todos"
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
      newTodoText: null,
      todos: []
    }
  },
  computed: {
    totalTodos () {
      return this.todos.length
    },
    today () {
      return new Date().toLocaleString('en-GB', { weekday: 'long', month: 'long', day: '2-digit' })
    }
  },

  methods: {
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
    height: 100%;
    padding: 1.5rem;
  }

  .headline {
    font-size: 1.5rem;
    margin: 0;
    margin-bottom: 1.5rem;

    &--4 {
      font-size: 1.15rem;
      font-weight: normal;
      color: rgba(black, 0.51);
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
  }
</style>
