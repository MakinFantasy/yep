<template>
  <button class="button">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: 'my-button',
};
</script>

<style scoped lang="scss">
@use '@/assets/scss/function' as *;
@use '@/assets/scss/variables' as *;
@use '@/assets/scss/mixin' as *;

.button {
  padding: 15px;
  color: color(button-text-color);
  background-color: color(button-color);
  border-radius: 10px;
  border: 1px solid color(button-border-color);
  transition: all 0.7s ease;
  @include media(min, md) {
    padding: 15px;
  }

  &:focus {
    box-shadow: 0px 0px 7px 3px #f5f5dc;
  }

  &:active {
    background-color: color(button-active);
  }

  &[disabled] {
    transform: scale(0.9);
    opacity: 0.7;
  }

  &[disabled]:active {
    background-color: color(disabled-color);
  }
}
</style>
