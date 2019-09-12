<template>
  <button
    :class="['button', { 'button--outlined': outlined }]"
    @click="handleClick"
  >
    <slot />
  </button>
</template>

<script>
export default {
  name: 'BaseButton',
  props: {
    outlined: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    handleClick (e) {
      this.$emit('click', e)
    }
  }
}
</script>

<style lang="scss">
  .button {
    -webkit-appearance: none;
    background-color: #ff0047;
    color: white;
    padding: 0.6rem 1rem;
    border-radius: 4px;
    border: 0;
    text-transform: uppercase;
    font-size: 0.8rem;
    transition: 0.2s opacity ease;

    &:focus {
      outline: 0;
      opacity: 0.9;
    }

    &:active {
      opacity: 0.5;
    }

    &:hover {
      cursor: pointer;
    }
    &--outlined {
      background-color: transparent;
      border: 1px solid #ff0047;
      color: #ff0047;
    }
  }
</style>
