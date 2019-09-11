<template>
  <div class="text-field">
    <label
      v-if="labelText"
      :for="id"
      class="text-field__label"
    >
      {{ labelText }}
    </label>
    <input
      v-bind="attrs"
      :id="id"
      :value="value"
      :type="type"
      class="text-field__input"
      v-on="listeners"
    >
  </div>
</template>

<script>
export default {
  name: 'BaseTextField',
  inheritAttrs: false,
  model: {
    event: 'model:input'
  },
  props: {
    value: {
      type: String,
      default: null
    },
    type: {
      type: String,
      default: 'text'
    },
    labelText: {
      tyep: String,
      default: null
    }
  },
  computed: {
    id () {
      return `text-field-${this._uid}`
    },
    attrs () {
      return this.$attrs
    },
    listeners () {
      return {
        ...this.$listeners,
        input: this.handleInput
      }
    }
  },
  methods: {
    handleInput (event) {
      this.$emit('input', event)
      this.$emit('model:input', event.target.value)
    }
  }
}
</script>

<style lang="scss">
  .text-field {
    display: flex;
    flex-flow: column;
    width: 100%;
    border: 1px solid #ccc;
    border-radius: 4px;

    &__label {
      color: rgba(black, 0.51);
      font-size: 0.875rem;
    }

    &__input {
      -webkit-appearance: none;
      padding: 0.875rem;
      background-color: transparent;
      border: 0;
      font-size: 1rem;
      width: 100%;

      &:focus {
        outline: 0;
      }

      &::placeholder {
        color: rgba(black, 0.51);
        font-size: 0.875rem;
      }
    }
  }
</style>
